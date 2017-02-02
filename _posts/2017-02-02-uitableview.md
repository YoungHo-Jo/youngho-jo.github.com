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
### 1. ViewController 상속받기
사용중인 viewController에 UITableViewDelegate와, UITableViewDataSource를 상속 받는다.

### 2. override할 메소드를 적어준다.
{% highlight swift %}
tableView(_: numberOfRowsInSection:)
	// 보여줄 아이템의 개수를 리턴하는 메소드
        
tableView(_: cellForRowAtIndexPath:)
	// 각 Row마다 보여줄 Cell 을 만들어 리턴하는 메소드
    
tableView(_: didSelectRowAtIndexPath:)
	// Row가 select되면 불려질 메소드
{% endhighlight %}

### 3. Outlet 연결하기
> tableView
> DataSource
> Delegate

DataSource와 Delegate는 tableView를 우클릭하면 나오는 화면에서 확인할 수 있다.

### 4. viewDidLoad() 수정하기
{% highlight swift %}
register(_:cellClass)
    // cell 에 사용할 class 를 identifier를 이용해서 정해준다.
    
// ex)
self.tableView.register(UITableViewCell.self, forCellReuseIdentifier: "cell")
{% endhighlight %}
    
### 5. items collection 을 만든다.

### 6. 2의 메소드 수정하기
{% highlight swift %}
tableView(_: numberOfRowsInSection)
   	// 5에서 만들어진 item 개수를 리턴하게 한다.
        
tableView(_: cellForRowAtIndexPath:)
	// cell 을 만든다    
// ex)
{
  var cell:UITableViewCell = self.tableView.dequeueReusableCellWithIdentifier("cell") as UITableViewCell // cell 을 만들고
  cell.textLabel?.text = self.items[indexPath.row] // cell textLabel 을 설정해서
  return cell // 리턴
}
    
tableView(_: didSelectRowAtIndexPath:) 
   	// cell 이 선택되었을때 하고싶은 동작을 여기에 구현한다
{% endhighlight %}        
        
### Custom Cell 만들어 적용하기

### 1. UITableCell 을 상속받는 class를 만든다.
{% highlight swift %}
class CustomCell: UITableCell {}
{% endhighlight %}
    
### 2. storyBoard에서 tableCell을 추가한다
storyBoard에서 custom cell 을 사용하고 싶은 tableView 내부에 tableViewCell 을 추가하여, 원하는 디자인으로 수정한다.

### 3. 디자인한 Cell 과 CustomCell과 연결한다.
CustomCell class 내부에 cell 마다 다르게 하고 싶은것을 연결한다.

### 4. ViewController에서 메소드를 수정한다.
{% highlight swift %}	
viewDidLoad()
	// register 클래스를 바꾸어 준다.
        
tableView(_:cellForRowAtIndexPath:)
	// CustomCell 앞의 방법과 비슷하게 만들어 주고
	// cell에 연결된 값을 수정하여 리턴시켜준다.
{% endhighlight %}
        

