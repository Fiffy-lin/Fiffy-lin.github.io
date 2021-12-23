---
layout: post
title: "Heterogeneous Graph Transformer"
subtitle: ""
date: 2021-12-23
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Paper"]
---

Transformer 结构在 NLP 领域内非常的火，归因于它的 Attention 机制，通过 Q,K 计算出 score 可以识别出不同位置信息的重要程度，再与 V 相乘完成信息的传递。对于图来说，不同起点在向终点更新信息的时候，重要程度也是不同的，异构图尤甚，不同种类的边甚至都不一样。这篇 paper 把 Attention 机制引入到了图神经网络，据说能取得不错的效果。

由于异构图的特点，我们需要为每种类型的三元组准备不同的 Q/K/V/A 映射矩阵。

```Python
for t in range(self.num_types):
    self.k_linears.append(nn.Linear(in_dim, out_dim))
    self.q_linears.append(nn.Linear(in_dim, out_dim))
    self.v_linears.append(nn.Linear(in_dim, out_dim))
    self.a_linears.append(nn.Linear(out_dim, out_dim))
```

消息传递是起点到终点的过程，我们的 Attention 计算的是不同起点对同一个终点的重要程度，因此 Query 是终点的信息，Key 是起点的信息。需要传递的是起点的信息，因此 Value 是起点的信息。

```Python
k_linear = self.k_linears[node_dict[srctype]]
v_linear = self.v_linears[node_dict[srctype]]
q_linear = self.q_linears[node_dict[dsttype]]

k = k_linear(h[srctype]).view(-1, self.n_heads, self.d_k)
v = v_linear(h[srctype]).view(-1, self.n_heads, self.d_k)
q = q_linear(h[dsttype][:sub_graph.number_of_dst_nodes()]).view(-1, self.n_heads, self.d_k)
```

异构图的一个特点便是，对某种确定的起点、终点类型组合，仍然可以存在多类边。因此计算 Attention Score 的时候，要考虑边类型的影响。同理，消息传递的时候，也要考虑边类型的影响。由于不同类型的边对终点的影响不一样，作者还加了一个 prior tensor 来缩放每种类型的边的重要程度。计算过程和传统的 Attention 差不多，Q/K 点积，缩放。

```Python
self.relation_pri = nn.Parameter(torch.ones(self.num_relations, self.n_heads))
self.relation_att = nn.Parameter(torch.Tensor(self.num_relations, n_heads, self.d_k, self.d_k))
self.relation_msg = nn.Parameter(torch.Tensor(self.num_relations, n_heads, self.d_k, self.d_k))
```

```Python
k = torch.einsum("bij,ijk->bik", k, relation_att)
v = torch.einsum("bij,ijk->bik", v, relation_msg)
sub_graph.srcdata['k'] = k
sub_graph.dstdata['q'] = q
sub_graph.srcdata['v_%d' % e_id] = v

sub_graph.apply_edges(fn.v_dot_u('q', 'k', 't'))
attn_score = sub_graph.edata.pop('t').sum(-1) * relation_pri / self.sqrt_dk
```

下面的 Softmax 略有不同。dgl 提供的 edge_softmax() 是对一种边类型实施的，但我们实际需要的是对某个终点的所有边进行 Softmax，来衡量每条边的重要程度，而不是每种类型分别计算。

```Python
# 对每种边单独存储结果
sub_graph.edata['score'] = torch.exp(attn_score.unsqueeze(-1))

# 对所有边进行 Softmax
G.multi_update_all({etype: (fn.copy_e('score', 'score_temp'), fn.sum('score_temp', 'score_sum')) for etype, _ in edge_dict.items()}, cross_reducer='sum')
for etype in G.canonical_etypes:
		G.apply_edges(fn.e_div_v('score', 'score_sum', 't'), etype=etype)
```

有了 Attention Score，就可以进行消息传递了。传递和传统 Attention 一样，就是分数乘上 Value

```Python
G.multi_update_all({etype: (fn.u_mul_e('v_%d' % e_id, 't', 'm'), fn.sum('m', 't')) for etype, e_id in edge_dict.items()}, cross_reducer='mean')
```

最后还剩下节点的更新过程。首先把计算完成的 Value 投影回节点信息空间，每种类型的 A 矩阵也是不一样的。

```Python
t = G.dstnodes[ntype].data['t'].view(-1, self.out_dim)
trans_out = self.drop(self.a_linears[n_id](t))
```

最后更新的时候加了一层残差连接，保留了一部分上一层的节点信息。这个保存的比例也是学出来的。

```Python
alpha = torch.sigmoid(self.skip[n_id])
trans_out = trans_out * alpha + h[ntype][:G.number_of_dst_nodes(ntype=ntype)] * (1 - alpha)
```

在知乎数据上训的结果还没出来，从训练时间上看，HGT 比 GraphSAGE 慢了一倍，希望结果能好点。
