# **文档开始说明**

1. 有一些类和方法名需要批量替换
2. 目前所有资源：图片、spine、龙骨、音效等全都在loading的时候缓存到了内存中，所以代码中异步加载和回调的方法需要修改为直接使用
（比如：egret.Event.COMPLETE 注册的回调方法等异步加载的地方）
3. 新增的代码和功能尽量不要再使用封装的东西，直接用cocos实现。


## **需要替换使用的：**

1. eui文件夹下Eui开头的脚本在旧代码中需要替换为Eui开头，比如eui.Label改为EuiLabel

2. bridge_iframe.sendMsgToGame('mqtt_reconnecting')
   事件派发改为 **NativePlatform._instance.sendMsgToGame('mqtt_reconnecting')** （监听的方法不变）。

3. **eui.list组件**：  
   只做了 **dataProvider** 和 **itemRenderer** 方法，列表项方法只实现了 **data** 和 **dataChanged**，如果不够用，后续再加。

4. **itemRenderer 使用方式**（代码里需把旧代码继承 **eui.ItemRenderer** 改为继承 **EuiItemRenderer**）：  
   1. 创建节点  
   2. 创建继承 **EuiItemRenderer** 的class  
   3. 在 class 中重写 **dataChanged** 方法  
   4. 脚本挂载到节点上  

5. **egret 的 touchTap 事件**，cocos 没有，使用 **touchEnd** 代替（多余的 **touchEnd** 监听删除）。

6. **egret 的 TOUCH_RELEASE_OUTSIDE 事件**，cocos 没有，使用 **touchCancel** 代替（多余的 **touchCancel** 监听删除）。

7. EUIShape.graphics 在CRUSH项目中需要终点关注，有些实现方法在cocos中不能实现。

8. Tween$ 没有做兼容封装，需批量替换为 egret.Tween

9. Eui组件，比如EuiImage、EuiGroup在节点上，点击事件的e.currentTarget需要替换为getTouchTarget(e.currentTarget) , 点击事件传的参数需要替换为getTouchEventTarget(e)



## **不需要替换使用的：**

1. **TileLayout** 使用不多，封装花时间，界面开发时直接使用 cocos的 **Layout** 功能替换。


## **还未处理的**
### **未处理的监听事件**
1. RES.addEventListener(RES.ResourceEvent.CONFIG_COMPLETE)
2. egret.Event.COMPLETE
3. egret.Event.CHANGE
4. egret.Event.ADDED_TO_STAGE
5. egret.Event.REMOVED_FROM_STAGE
6. egret.Event.RESIZE
7. egret.Event.FOCUS_OUT
8. egret.Event.REMOVED
9. egret.IOErrorEvent.IO_ERROR
10. egret.TimerEvent.TIMER
11. egret.TextEvent.LINK
12. eui.ItemTapEvent.ITEM_TAP
13. eui.UIEvent.CHANGE_END
