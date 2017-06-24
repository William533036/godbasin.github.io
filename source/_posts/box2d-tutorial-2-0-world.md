---
title: Box2DJS教程2--物理世界
date: 2017-02-17 21:00:06
categories: box2djs方糖
tags: 教程
---
第二章介绍物理世界，包括世界初始化类，以及创建一个世界。
<!--more-->

## 物理世界
---
### 世界初始化
每个Box2D程序都是从一个世界对象(world object)的创建开始的，它是一个管理内存，对象和模拟的中心，在这个世界中，刚体将遵循物理规律运动。

所以，我们首先需要创建一个世界，世界初始化类即用于创建一个世界对象，并设定有关世界的初始参数。

- 创建物理世界

利用Box2D开发程序时，首先要创建一个世界对象。它负责管理内部一切对象的内存和模拟过程。

- 添加物理边界

要创建一个世界中的对象，首先要为世界定义边界区域，Box2D针对区域内的所有对象进行模拟碰撞，区域的大小并不重要，但更适合的区域将提高程序性能，一般来讲这个区域设置的要比演示区域更大一些，因为一旦对象在运动时到达了边界，它就会被“冻结”并停止一切模拟活动。

- 添加基本力--重力

然后要为这个世界设置重力，用向量`b2Vec2(x,y)`来表示的，x代表水平运动，正数向右，负数向左，y代表垂直运动，正数向下，负数向上；

同时需要定义一个布尔型参数用来表示是否允许睡眠，在这个世界中生成的一切对象的模拟效果都是实时计算出来的，当doSleep=false的时候，即使物体停止了运动，计算机还是在不停的进行着运算，其实这是完全不必要的，所以一般都设为true，这样当物体停止之后就不会进行无谓的cpu消耗了。

- 添加物理对象

参数都准备好之后，传入b2World对象中并将其实例化，这样一个物理引擎的模拟区域就做好了，可以开始加入模拟的对象了。
基本步骤为：
1. 创建并定义刚体位置；
2. 给刚体定义皮肤；
3. 用世界对象添加刚体实例;
4. 根据皮肤形状创建模拟图形类：摩擦力、密度、弹力等等；
5. 在刚体上添加模拟图形实例；
6. 根据刚体的密度和面积计算出质量，密度*面积=质量。

- 更新状态

然后，就可以通过监听帧频率而不断刷新实现让所有对象模拟运动，把上面那两个参数传入世界对象的Step方法中即可，同时我们需要遍历世界中的一切对象，并对每个对象的坐标和角度进行更新。

## b2World
---
### 介绍
b2World类包含着物体和关节。它管理着模拟的方方面面，并允许异步查询(就像AABB查询)。你与Box2D的大部分交互都将通过b2World对象来完成。

创建一个世界十分的简单。你只需提供一个包围盒和一个重力向量。

轴对齐包围盒(AABB)应该包围着世界。稍微比世界大一些的包围盒可以提升性能，比方2倍大小才安全。如果你的许多物体都掉进了深渊，你应该侦测并移除它们。这能提升性能并预防浮点溢出。

### 创建一个世界
创建一个世界，并设置其有效区域的大小，重力大小及方向，以及是否允许休眠等。

1. 定义有效区域大小：创建一个`b2AABB`类，然后设定其左上角及右下角坐标。有效区域即box2d可以发挥作用，反映各种物理规律的区域。

``` javascript
worldAABB = new b2AABB();
worldAABB.minVertex.Set(-1000, -1000);
worldAABB.maxVertex.Set(1200, 1000);
```

注意 ： worldAABB应该永远比物体所在的区域要大，让worldAABB更大总比太小要好。如果一个物体到达了worldAABB的边界，它就会被冻结并停止模拟。

2. 设置重力大小及方向：通过定义一个二维矢量来实现，创建一个`b2Vec2`类，并在括号中给出其x，y方向的大小。

``` javascript
gravity = new b2Vec2(0, 1000);
```

备注：gravity是重力加速度。

3. 设置世界是否会休眠：true即允许休眠，false不允许休眠。当世界被设置为允许休眠时，物体静止一段时间后会被判定为休眠，直到有碰撞发生或者人为激活。一个休眠中的物体不需要任何模拟。

``` javascript
var doSleep = true;
```

4. 创建世界：利用`b2World`创建拥有上述性质的世界。

``` javascript
world = new b2World(worldAABB, gravity, doSleep);
```

### 创建一个世界的完整代码
``` javascript
var worldAABB = new b2AABB();
worldAABB.minVertex.Set(-1000, -1000);
worldAABB.maxVertex.Set(1000, 1000);
var gravity = new b2Vec2(0, 300);
var doSleep = true;
var world = new b2World(worldAABB, gravity, doSleep);
```


### [返回总目录](/2017/02/17/box2d-tutorial-0-catalog/) 