# js实现2048小游戏 🎮

### 19计算机二班19219111241董浩雄移动应用开发个人项目

演示地址：https://bulingxiong.github.io/Final-homework_2048game/

B站视频教程：https://www.bilibili.com/video/BV1Hb411L729?spm_id_from=333.337.search-card.all.click

源代码：https://github.com/bulingxiong/Final-homework_2048game

## 1、游戏简介

2048是一款休闲益智类的数字叠加小游戏，游戏代码适配网页和移动端。

## 2、游戏玩法

在 4*4 的16宫格中，您可以选择上、下、左、右四个方向进行操作，数字会按方向移动，相邻的两个数字相同就会合并，组成更大的数字，每次移动或合并后会自动增加一个数字。

当16宫格中没有空格子，且四个方向都无法操作时，游戏结束。

## 3、游戏目的

目的是合并出 2048 这个数字，获得更高的分数。

## 4、游戏截图

![image](https://user-images.githubusercontent.com/105336974/170182036-222f135f-1f02-40b1-9370-f5bbb3973e48.png)
## 5、游戏实现原理

### (1)首先，把16宫格看成是矩阵的形式

![image](https://user-images.githubusercontent.com/105336974/170182018-661b925c-6db9-435f-aac5-7c4f34a01e7f.png)
### (2)在html中给每个格子添加类别及ID，来记录每个格子的位置

![image](https://user-images.githubusercontent.com/105336974/170182077-c4ea9b79-fc2f-45ed-a9d0-80417ef23722.png)

### (3)游戏开始时，随机生成两个数字，2或者4，出现在矩阵中任意位置

~~~javascript
```// 在十六个格子里找一个空闲的生成一个数字
function generateOneNumber() {
    // 判断是否有空格子
    if (nospace(board))
        return false;
    // 随机一个位置
    var randx = getRand44Num();
    var randy = getRand44Num();
    // 判断该位置是否可用
    var times = 0;
    // 优化随机数生成算法，只让计算机猜五十次，看看能不能生成一个新的数字的位置
    while (times < 50) {
        if (board[randx][randy] == 0)
            break;

        var randx = getRand44Num();
        var randy = getRand44Num();
        times++;
    }
    // 人工的生成一个新的数字的位置
    if (times == 50) {
        for (var i = 0; i < 4; i++) {
            for (var j = 0; j < 4; j++) {
                if (board[i][j] == 0) {
                    randx = i;
                    randy = j;
                }

            }
        }
    }

    // 随机一个数字
    var randNumber = getRand24Num();
    // 在随机位置显示随机数字
    board[randx][randy] = randNumber;
    showNumberWithAnimation(randx, randy, randNumber);
    return true;
}
```
~~~

### (4)游戏的核心在于移动

移动有四个方向：上、下、左、右。实现思路如下：

```
如果触发向左移动
　　遍历所有非空元素
　　　　如果当前元素在第一个位置
           不动
　　　　如果当前元素不在第一个位置
　　　　　　如果当前元素左侧是空元素    
              向左移动
　　　　　　如果当前元素左侧是非空元素    
　　　　　　　　如果左侧元素和当前元素的内容不同    
                  不动
　　　　　　　　如果左侧元素和当前元素的内容相同    
                  向左合并
 

如果触发向右移动
　　遍历所有非空元素
　　　　如果当前元素在最后一个位置     
           不动
　　　　如果当前元素不在最后一个位置
　　　　　　如果当前元素右侧是空元素   
              向右移动
　　　　　　如果当前元素右侧是非空元素    
　　　　　　　　如果右侧元素和当前元素的内容不同    
                  不动
　　　　　　　　如果右侧元素和当前元素的内容相同    
                  向右合并
```

向上移动 和 向下移动的思路同上。

### (5)判断游戏是否结束

```
如果没有空格子并且无法移动 则Game Over! 
function isgameover() {
    if (nospace(board) && nomove(board)) {
        gameover();
    }
}

function gameover() {
    alert("game over!");
}

```

