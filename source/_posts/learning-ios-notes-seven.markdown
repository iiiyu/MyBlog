---
layout: post
title: "iOS笔记 (7)"
date: 2012-05-06 10:28
comments: true
tags: iOS
---

# iOS笔记 Core Location定位


## 主要问题
很多应用上需要获得当前用户所在的位置，有或者需要在地图上根据座标显示一些点一些信息。这次主要分享Core Location定位当前位置我遇到的问题。


<!--more-->

## 第一个方案
根据我看的文档所示定位分为一下几个步骤：

* 包含Core Location framework
* 创建一个CLLocationManager的实例
* 设置CLLocationManagerDelegate委托
* 实现CLLocationManagerDelegate委托里面的- (void)locationManager:(CLLocationManager *)manager didUpdateToLocation:(CLLocation *)newLocation fromLocation:(CLLocation *)oldLocation方法

其中原理就不细说了。只说实现问题。在完成了- (void)locationManager:(CLLocationManager *)manager didUpdateToLocation:(CLLocation *)newLocation fromLocation:(CLLocation *)oldLocation以后。当我们想要定位的时候。需要启动startUpdatingLocation。因为启用定位是一个耗电的工程。如果时刻定位的话，程序会变成电池杀手。所以，各种大牛建议，定位只在需要的时候定位。完成以后我们应该用停止它stopUpdatingLocation。

调用你实例的CLLocationManager startUpdatingLocation以后，会轮询- (void)locationManager:(CLLocationManager *)manager didUpdateToLocation:(CLLocation *)newLocation fromLocation:(CLLocation *)oldLocation方法。直到stopUpdatingLocation为止。因此可以用一个数组存储一定的newLocation。然后取平均值。本来我以为这样可能会取值准确一些。但是我实际定位估计有2-3km的距离。起初我以为是用ip来定位可能误差较大没有在意(用的iPad wifi测试).后来用其他App看到定位很准确，没有这么大的误差,就觉得做错了。然后尝试第二种方案。

## 第二个方案
隐约记得有种叫火星座标的东西。在[这里V2EX](http://www.v2ex.com/t/11666)普及了一下知识以后，觉得难道是火星人搞鬼了。然后纠结于解决方法。最后忘记在哪里看到MKMapView里面有一个userLocation的属性。就应该是当前位置座标。干净尝试一下:

* 包含MapKit framework
* 创建MKMapView一个实例类
* 设置MKMapViewDelegate委托
* 实现MKMapViewDelegate委托中的- (void)mapView:(MKMapView *)mapView didUpdateUserLocation:(MKUserLocation *)userLocation
方法
* MKMapView实例调用setShowsUserLocation



这样在- (void)mapView:(MKMapView *)mapView didUpdateUserLocation:(MKUserLocation *)userLocation方法里面userLocation就可以取得当前用户的位置。而且很准确。


## 总结
我不知道第一个方案为什么不准确，而第二个比较准确。跟曹哥讨论以后，曹哥告诉我说有人为了获得位置可能会建立一个不显示的MKMapView而仅仅是为了获得坐标。特此记录。



