---
layout: post
published: true
date: '2017-02-02 17:29 +0900'
modified: '2017-02-02 17:29 +0900'
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: UITableView
description: UITableView 생성 및 활용하기
category: info
tags:
  - Swift
---
## CocoaTouch/Swift) UITableView

### 기본적인 UITableView 다루는 방법
#### 1. ViewController 상속받기
사용중인 viewController에 UITableViewDelegate와, UITableViewDataSource를 상속 받는다.

#### 2. override할 메소드를 적어준다.
{% highlight swift %}
tableView(_: numberOfRowsInSection:)
    // 보여줄 아이템의 개수를 리턴하는 메소드
        
tableView(_: cellForRowAtIndexPath:)
	// 각 Row마다 보여줄 Cell 을 만들어 리턴하는 메소드
    
tableView(_: didSelectRowAtIndexPath:)
	// Row가 select되면 불려질 메소드
{% endhighlight %}