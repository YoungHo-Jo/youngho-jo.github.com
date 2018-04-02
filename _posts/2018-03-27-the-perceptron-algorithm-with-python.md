---
layout: post
published: true
date: '2018-03-27 18:05 +0900'
modified: '2018-03-27 18:05 +0900'
category: base
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: The Perceptron Algorithm with Python
tags:
  - default
  - Perceptron
  - AI
  - Python
---
출처:
https://machinelearningmastery.com/implement-perceptron-algorithm-scratch-python/
https://pythonmachinelearning.pro/perceptrons-the-first-neural-networks/


# Perceptron Algorithm
간단한 Artifical Neural Network

## Term
**Neuron
**Layer** Neuron 집합
**Perceptron** Layer집합, Neural Network

## Biological Neurons
Binary Inputs 받음
하지만, Weight 된 만큼 각 Input들이 다루어 짐
Weighted Signals 을 합쳐 Threshold를 넘으면 Fire
이 OUput은 axon을 거쳐 다른 Nerons로 감

## Artifical Neurons
![스크린샷 2018-03-28 오후 3.38.52.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 3.38.52.png)
binary inputs이 들어오고 weights와 곱해져서 더해짐
z 를 ****pre-activation**** 이라함
![스크린샷 2018-03-28 오후 3.40.52.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 3.40.52.png)
****bias**** = b (contant factor)
![스크린샷 2018-03-28 오후 3.40.26.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 3.40.26.png)
편의상 bias와 weight vector를 합친다.

weighted sum을 구한후, ****activation function****을 적용한다. ****(step function)****
![스크린샷 2018-03-28 오후 3.44.17.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 3.44.17.png)
하나의 Neuron에 적용된다. 
1이 나온다는 것은 Biological Neuron에서 Fire을 의미한다.
하나의 Neural Network에 적용하는 식은 다음과 같다.
![스크린샷 2018-03-28 오후 3.45.52.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 3.45.52.png)

## Capabilities and Limitations of Perceptrons
Binary Input이기 때문에 Binary Classification에 사용 가능하다.
즉, 오직 하나 혹 두 class에 속한다. 즉 logic gate 처럼 가능

한계점으로 선형적으로 분리된 경우에만 적용가능하다.
선형적으로 분리된 경우: 두 class를 분리하는 한 선을 그을 수 있는 경우 => ****Decision Boundary****

logic gate의 경우 AND OR 은 가능하나, XOR은 불가능(single layer 형태의 경우)

이는 위의 Weighted Sum 식을 보면 알 수 있다. => 선형식 처럼 생김 ****hyperplan****

따라서, ****Hidden Layer / Multiple Layers****을 사용하면 XOR 문제를 해결 가능하다.


## How to Train Perceptrons
기대와 실제가 다를때 Weight Vector를 수정하여 조정한다.
Weight Vector는 Perceptron 의 parameter이기 때문에 이 값을 계속해서 수정해서
원하는 경과를 얻어내야 한다.

![스크린샷 2018-03-28 오후 4.16.12.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 4.16.12.png)
좋은 delta w를 정해야한다. 

****Error****의 경우 다음과 같이 정의할 수 있다.
![스크린샷 2018-03-28 오후 4.17.05.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 4.17.05.png)
Error = Desired Output - Predicted Output
![스크린샷 2018-03-28 오후 4.18.24.png]({{site.baseurl}}/images/media/스크린샷 2018-03-28 오후 4.18.24.png)
새 Weight = 기존 Weight + Hyperparameter(Leraing Rate) * Error * x

****Epoch*** one epoch는 Perceptron이 Training data를 한 번 훑었을 때를 말한다.
