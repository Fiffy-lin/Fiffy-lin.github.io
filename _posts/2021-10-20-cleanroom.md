---
layout: post
title: "刷题记录-扫地机器人"
subtitle: ""
date: 2021-10-20
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","DFS"]
---

# 题目

房间（用格栅表示）中有一个扫地机器人。格栅中的每一个格子有空和障碍物两种可能。

扫地机器人提供4个API，可以向前进，向左转或者向右转。每次转弯90度。

当扫地机器人试图进入障碍物格子时，它的碰撞传感器会探测出障碍物，使它停留在原地。

请利用提供的4个API编写让机器人清理整个房间的算法。

```
interface Robot {
  // 若下一个方格为空，则返回true，并移动至该方格
  // 若下一个方格为障碍物，则返回false，并停留在原地
  boolean move();

  // 在调用turnLeft/turnRight后机器人会停留在原位置
  // 每次转弯90度
  void turnLeft();
  void turnRight();

  // 清理所在方格
  void clean();
}
```

示例:

```
输入:
room = [
  [1,1,1,1,1,0,1,1],
  [1,1,1,1,1,0,1,1],
  [1,0,1,1,1,1,1,1],
  [0,0,0,1,0,0,0,0],
  [1,1,1,1,1,1,1,1]
],
row = 1,
col = 3

解析:
房间格栅用0或1填充。0表示障碍物，1表示可以通过。
机器人从row=1，col=3的初始位置出发。在左上角的一行以下，三列以右。
```

注意:

- 输入只用于初始化房间和机器人的位置。你需要“盲解”这个问题。换而言之，你必须在对房间和机器人位置一无所知的情况下，只使用4个给出的API解决问题。 
- 扫地机器人的初始位置一定是空地。
- 扫地机器人的初始方向向上。
- 所有可抵达的格子都是相连的，亦即所有标记为1的格子机器人都可以抵达。
- 可以假定格栅的四周都被墙包围。

# 思路

模拟连续的清扫过程，就是碰到墙或者扫到的就转向，如果四个方向都不行，就回溯一步。

# 代码

```c++
class Solution {
public:
  	//这里得写一个二维 vector dirs，但是因为 github pages 渲染会报错，先删掉了
    void dfs(Robot& robot, unordered_set<string>& visited, int x, int y, int dir){
        robot.clean();//清扫
        for(int i = 1; i <= 4; i++){//转四次后返回进入函数的初始状态
            robot.turnRight();//右转
            int new_dir = (dir + i) % 4;//下一步的方向
            int xx = x + dirs[new_dir][0];//下一步位置x
            int yy = y + dirs[new_dir][1];//下一步位置y
            string next = to_string(xx) + ',' + to_string(yy);
            if(visited.find(next) != visited.end())
                continue;
            visited.insert(next);//记录探查过
            if(robot.move()){//是空地
                dfs(robot, visited, xx, yy, new_dir);//继续扫
                //恢复原状
                robot.turnRight();
                robot.turnRight();//向后转
                robot.move();//前进
                robot.turnRight();
                robot.turnRight();//向后转
            }
        }
    }
    void cleanRoom(Robot& robot) {
        unordered_set<string> visited;
        visited.insert("0,0");
        dfs(robot, visited, 0, 0, 0);
    }
};
```

