# 坦克大战？

一个随便写写的文档，这原来是计划做基于 nebulas 的 dapp 游戏，但是没有过审，脑壳疼...现在改为了离线版，不能上传代码和对战。（其实对战功能也不完善 23333

写完代码后 点开始测试可以开始游戏，可以和我编写的智障程序对战...

这个项目已经鸽了很久（反正也没人玩 2333）后续心情好会重够下代码（代码烂的已经没法写了），修改下游戏的玩法.对游戏感兴趣或者有什么建议可以邮箱联系我 ilove111219@gmail.com

## 吐槽 啊~~

代码可以说是一团糟...嫌麻烦直用一个 index.html 写的...结果越写越麻烦...早知道用 react 了....

## 游戏的机制

### 观察对象

你会实时的获得一个`ob`对象,这是一个观察列表，这个列表里包含了你看到了所有信息包括几大类

1.  边界-border：就是你离边界多远，在实战中当 border 小于一个值的时候，玩家就应该控制转向了.

```js
ob.get('border'); // => [{d:100}] #距离墙60m
```

2.  敌人-enemy：比如你的角度是（与正东方向的夹角）45°，视野角度是 60°，那么你可以观察到 15°-75° 的敌人。比如你的 46° 100m 有个敌人，50° 200m 有个敌人，180° 100m 有个敌人，你将会得到一个这样的信息

```js
ob.get('enemy'); //=> [{a:20,d:200},{a:10,d:100}] #敌人列表:[{方向:20°,距离:100m},{方向:10°,距离:100m}]
```

180° 的敌人不在你的观察范围，所以你得不到背后敌人的信息。你可以调整自己的角度去发射炮弹攻击敌人

3- 损伤-damage: 返回一个不精确的方向，比如前后左右，这样被打的时候可以大概知道敌人的方向。做出反击。同理在攻击敌人时也应该变攻击边移动。

### 玩家可以的操作

```js
tank.do('停止');
tank.do('前进');
tank.do('后退');
tank.do('左拐');
tank.do('右拐');
tank.do('原地左转');
tank.do('原地右转');
tank.do('炮台左转'); //暂时没有做炮台朝向，炮台朝向和坦克朝向相同
tank.do('炮台右转');
tank.do('开火');

//english
tank.do('stop');
tank.do('forward');
tank.do('back');
tank.do('rotatLeft');
tank.do('rotatRight');
tank.do('spinLeft');
tank.do('spinRight');
tank.do('batteryRotatLeft');
tank.do('batteryRotatRight');
tank.do('shooting');
```

### 示范代码

当 border<150m 时原地左转

```js
if (ob.get('border') && ob.get('border')[0].d < 150) {
  tank.do('原地左转');
} else {
  tank.do('前进');
}
```

对敌人

```js
//  d为距离 a为方向
//  ob.get('border') 返回的是一个数组，因为border只有一个，所以取第0个，
//  ob.get('enemy') 返回的也是一个数组，因为enemy可能有多个，所以应该对数组进行遍历

if (ob.get('enemy').length > 0) {
  tank.do('开火');
} else if (ob.get('border') && ob.get('border')[0].d < 150) {
  tank.do('原地左转');
} else {
  tank.do('前进');
}
```
