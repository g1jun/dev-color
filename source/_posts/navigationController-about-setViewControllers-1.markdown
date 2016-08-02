---
title: navigationController页面的跳转技巧--setViewControllers
date: 2016-08-02 16:19:05
前言:
---
相信大家对navigationController的跳转都不陌生，UINavigationController是实现画面多层次跳转，并且可以自动地记忆跳转所经过的路径，按照这些记录的路径信息，可以依次返回到上层画面中（即支持返回按钮）。
##### 我在开发中遇到这样的需求，由A页面跳转到B页面返回时要返回到之前没有创建的C页面。

    
<!-- more -->

## 一、关于跳转
在使用UINavigationController时我们用的最多的就是Push和pop方法，而pop方法又包括popViewControllerAnimated、popToViewController与popToRootViewControllerAnimated，分别是返回上一层，返回指定层与返回根视图（即UINavigationController最开始的那一层）
我们在运用这些跳转时，每push一层都会将一个页面保存到self.navigationController.viewControllers里,相应的pop就是将页面从self.navigationController.viewControllers移除的过程。
## 二、分析过程

开始我的做法很简单，觉得在返回时pop掉B页面

	self.navigationController.popViewControllerAnimated(false)

然后push新页面C

	self.navigationController.pushViewController(C, animated: true)

这种看似合理的方法其实也有问题，就是即使pop时关闭了动画，A页面依然会闪现一下
于是我想到了新的思路，我们可以对self.navigationController.viewControllers进行操作，因为viewControllers: [UIViewController]，就是一个数组。
我将这个数组取出对他进行操作，再赋值给他是不是也可以呢

	var controllerArr = self.navigationController?.viewControllers
	controllerArr?.insert(C, atIndex:controllerArr?.count-2)
	self.navigationController?.viewControllers = controllerArr
	self.navigationController?.popViewControllerAnimated(true)
                        
   效果完成了需求中的效果，但是发现B页面依然在self.navigationController?.viewControllers中储存的Controller中，导致数据在传输时数据会出现影响。后来当我看到setViewControllers这个方法时，突然觉得刚才那个就是个错。
   
  	 public func setViewControllers(viewControllers: [UIViewController], animated: Bool)
  	 // If animated is YES, then simulate a push or pop depending on whether the new top view controller was previously in the stack.

   即从iPhone OS 3.0以后，可以通过调用setViewController方法将画面的跳转历史路径（堆栈）完全替换，（这个才是正解😂）
   那么setViewController要怎么用呢，看到网上还有不少人使用错误，下面举两个例子，可以参考
   
### 1. B页面返回时跳转新页面C

{% img /images/article/setviewcontrollers跳转/跳转1.png %}

	var controllerArr = self.navigationController?.viewControllers//获取Controller数组
	controllerArr?.removeAll()//移除controllerArr中保存的历史路径
	//重新添加新的路径
	controllerArr?.append(self.navigationController?.viewControllers[0])
	controllerArr?.append(C)
	//将组建好的新的跳转路径 set进self.navigationController里
	self.navigationController?.setViewControllers(controllerArr!, animated: true)//这里直接setViewControllers即可，不需要push或者pop方法
	
### 2. A页面跳转到B页面时，添加新页面C
（即在B页面加到navigationController里之前修改页面路径）

{% img /images/article/setviewcontrollers跳转/跳转2.png %}

	var controllerArr = self.navigationController?.viewControllers//获取Controller数组
	controllerArr?.removeAll()//移除controllerArr中保存的历史路径
	//重新添加新的路径
	controllerArr?.append(self.navigationController?.viewControllers[0])
	controllerArr?.append(C)
	controllerArr?.append(B)
	//这时历史路径为（root -> c -> b）
	//将组建好的新的跳转路径 set进self.navigationController里
	self.navigationController?.setViewControllers(controllerArr!, animated: true)//直接写入，完成跳转B页面的同时修改了之前的跳转路径
然后在B页面里就可以直接返回pop上一层，就是想要的C页面

#### 小结
上面的两种方法，分别对应B页面需要对C页面数据操作的和不需要对C页面数据操作的两种情况，针对setViewControllers方法我们还可以有更多的操作满足各种奇葩的跳转，希望本文可以给你一些帮助和提示