---
layout: single
published: true
date: '2017-02-03 16:14 +0900'
modified: '2017-02-03 16:14 +0900'
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: App Development with Swift) QuestionBot2 구성 파헤치기
description: iBooks - App Development with Swift
tags:
  - Swift
toc: true
---
## iBooks - "App Development with Swift"
Apple에서 제작한 책으로, iBooks Store에서 다운받을 수 있다.

## QuestionBot2
책의 예시 프로젝트로, [여기서](https://developer.apple.com/sample-code/swift/downloads/app-dev-curriculum.zip) 책의 전체 예시를 다운받을 수 있다.

### UI 구성
#### Main.stroyboard
****Navigation Controller****를 시작으로, ****TableView Controller****를 담고 있다.
****TableView****와 ****CustomCell****을 담고 있다.

#### LaunchScreen.storyboard
엽을 처음 켰을 때 보여줄 화면 스크린으로, 현재는 비워져 있다.

#### ThinkingCell.swift
TableView의 CustomCell로, QuestionBot이 생각중임을 나타내는 Cell이다.

#### ConversationCell.swift
QuestionBot의 대답을 표시할 CustomCell이다.

#### AskCell.swift
사용자 질문을 적을 CustomCell이다.

#### Assets.xcassets
이 앱에서 사용중인 이미지들을 관리한다.

### Controller 구성
#### ConverstaionViewController.swift
Main.stroyboard의 TableView를 담고있는 Controller이다.
사용자의 질문과 QuestionBot의 대답을 상황에 맞게 표시해주는 역할을 한다.

### Model 구성
#### Message.swift
다른 swift 파일에서 사용할 (enum)****MessageType****과 (sturct)****Message****를 정의하고 있다.

#### QuestionAnswerer.swift
유저로부터 질문을 받았을 때, 질문에 해당하는 답변을 처리하는 (struct)****QeustionAnswerer****를 정의한다.
이 struct는 한 개의 (method)****responseTo(question:)****을 정의한다.
이 메소드는,
1. 질문의 첫 단어가 "hello" 일 때, "why, hello there!"
2. 질문이 "where are the cookies?" 일 때, "In the cookie jar!"
3. 질문의 첫 단어가 "where" 일 때, "To the North!"
4. 위 세 가지 상황에 맞지않는 질문일 땐, 질문 길이를 mod 3하여 나오는 값으로 대답의 종류르 3가지로 구분하여 대답한다.

#### ConversationDataSource.swift
(class)****ConversationDataSource****를 정의한다.
메시지의 총 갯수와, 질문과 답을 add하는 등의 메소드를 제공하고있으며,
현재 상태에서는 큰 역할을 하지 않는다.

### 앱 동작 그림
![questionBot2_working_diagram.png]({{site.baseurl}}/images/media/questionBot2_working_diagram.png)

### ConversationViewController을 자세히 알아보자
{% highlight swift %}
class ConversationViewController: UITableViewController {}
{% endhighlight %}

****TableView****를 컨트롤 하기 위해서, ****UITableViewController****를 상속받았다.

{% highlight swift %}
let questionAnswerer = QuestionAnswerer() // Struct
let conversationSource = ConversationDataSource() // Class
{% endhighlight %}

****questionAnwserer****는 질문에 답하기 위한 method를 가지고 있고, ****conversationSource****는 대화의 내용들을 저장하기위해 생성한다.

{% highlight swift %}
fileprivate func respondToQuestion(_ questionText: String) {
	
}
{% endhighlight %}


작성중 ...
