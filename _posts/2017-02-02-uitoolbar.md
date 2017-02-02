---
layout: post
published: true
date: '2017-02-02 16:58 +0900'
modified: '2017-02-02 16:58 +0900'
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: UIToolbar
description: UIToolbar
tags:
  - Swift
---
## Cocoa Touch - Swift) UIToolbar

### toolbar item 만들기
	UIButtonItems

### toolbar item 설정
	setItems(_:animated:)

### Customizing 하기
	기본적으로 appearance proxy에 setter 메세지를 전달해야 한다.
    또한, 디폴트 화면 방향을 정해야 한다.
    
### Configuring Toolbar Items
	var items: [UIBarButtonItems]?
		Toolbar에 표시될 아이템들
        
	func setItems([UIBarButtonItem]?, animated: Bool)
		Toolbar에 표시될 아이템을 정한다(애니메이션 지정가능)

### Customizing Appearance
	var barStyle: UIBarStyle
		모습을 나타낼 Toolbar Style
        
	var barTintColor: UIColor?
		toolbar background 에 적용할 색
        
	var tintColor: UIColor!
		bar button 아이템들에 적용할 색
        
	var isTranslucent: Bool
		toolbar를 반투명으로 할 것인가
        
	func backgroundImage(forToolbarPosition: UIBarPosition, barMetrics: UIBarMetrics)
		background로 사용할 image 리턴
        
	func setBackgroundImage(UIImage?, forToolbarPosition: UIBarPosition, barMetrics: UIBarMetrics)
		background에 image 적용
        
	func shadowImage(forToolbarPosition: UIBarPosition)
		toolbar 그림자에 사용할 image 리턴
        
	func setShadowImage(UIImage?, forToolbarPosition: UIBarPosition)
		toolbar 그림자에 사용할 image 적용
		
### Managing the Delegate
	var delegate: UIToolbarDelegate?
		The toolbar’s delegate object.

### 참고사항
Radio Button 형태로 디자인하고 싶다면, UITabBar를 활용하자!
