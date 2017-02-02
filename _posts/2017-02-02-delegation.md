---
layout: post
published: true
date: '2017-02-02 22:44 +0900'
modified: '2017-02-02 22:44 +0900'
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: Delegation
tags:
  - Swift
---
## Cocoa Core Competencies) Delegation
참고자료: [Apple](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14-SW2)

### 정의
Delegation은 object가 작동하고, 상호작용하는 간단하고 강력한 pattern 이다.
delegating object는 다른 object의 참조를 유지하고, 적절한 시간에 해당 object에 메시지를 보낸다.
이 메시지는 delegating object가 처리하거나, 처리하려고하는 event를 delegate에게 알려준다.
delegation은 그에 대한 반응으로 형태나, 상태를 업데이트하게 된다.
delegation의 주된 가치는 하나의 중심 object에서 다른 여러 objects의 동작을 customize할 수 있다는 것이다.

### Delegation과 Cocoa Frameworks
delegating object는 framework object이고, delegate는 custom controller의 object이다. 

### delegating object의 예시: NSWindow
Appkit framework의 NSWindow는 <strong>windowShouldClose:</strong> protocol을 지닌다.
만약, 닫기 버튼을 눌렀을 때, window object는 delegate에 <strong>windowShouldClose:</strong>를 전달하고, Boolean 값을 받아온다.
그에 따라서 window object가 처리되는것이다.

### Delegation and Notifications


작성중...



