---
layout: post
published: true
date: '2018-04-09 19:10 +0900'
modified: '2018-04-09 19:10 +0900'
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: Genetic Algorithm using DEAP
category: algorithm
tags:
  - python
  - deap
  - genetic algorithm
toc: true
---
## Overview
유연성 제공

## Types
필요에 맞게 정의할 수 있다.

`creator`

```python
from deap import base, creator
creator.create('FitnessmMin', base.Fitness, weights=(-1, 0, ))
creator.create('Individual', list, fitness=creator.FitnessMin)
```
    

## Initialization
타입 생성수 랜덤 값을 넣는 과정

`base.Toolbox()`

initilizer를 담고 있는 Container

```python
import random
from deap import tools

IND_SIZE = 10 # indivisual size

toolbox = base.Toolbox()
toolbox.register('attribute', random.random) # random 
# indivisual 생성
toolbox.register('indivisual', tools.initRepeat, 
				creator.Individual, toolbox.attribute, n=IND_SIZE) 
 # population 생성
toolbox.register('population', tools.initRepeat,
				list, toolbox.individual)
```


## Operators
initializer 와 비슷하나, tools 모듈에 이미 정의

원하는 것을 찾고 toolbox에 등록

```python
def evalutae(indivisual): # 사용자 정의 Evalutation
	return sum(indivisual), # fitness 값은 iterable해야하기 때문에 tuple 반환
    
toolbox.register('mate', tools.cxTwoPoint)
toolbox.register('mutate', tools.mutGaussian, mu=0, sigma=1, indpb=0.1)
toolbox.register('select', tools.selTournament, tournsize=3)
toolbox.register('evalutate', evaluate) 
# 이름은 toolbox에 의해 재정의됨 (GA가 Operator이름에 의존적이지 않게)
```

## Algorithms
main function 을 작성

```python
def main():
	pop = toolbox.population(n=50) # Population
   	CXPB, MUTPB, NGEN = 0.5, 0.2, 40 
    # Crossover probability, Mutation probability, Number of Generation
    
	# Evaluate the entire population
    fitnesses = map(toolbox.evaluate, pop)
	for individual, fitness in zip(pop, fitnesses):
    	individual.fitness.values = fitness
       
    for generation in range(NGEN):
    	# Select the next generation individuals
       	offspring = toolbox.select(pop, len(pop))
	    # Clone the selected individuals
    	offspring = map(toolbox.clone, offspring)
        
        #### map object => list 
        offspring = list(offspring)
        
        
       	# Apply crossover and mutation on the offspring
	    for child1, child2 in zip(offspring[::2], offspring[1::2]):
    	    if rarndom.random() < CXPB:
        	    toolbox.mate(child1, child2)
            	del child1.fitness.values
                del chile2.fitness.values
		
    	for mutantt in offspring:
			if random.random() < MUTPB:
            	toolbox.mutate(mutant)
                del mutant.fitness.values
              
		# Evaluate the individuals with an invalid fitness
    	invaliIndividual = [indivisual for indi in offspring if not indi.fitness.valid]
        fitnesses = map(toolbox.evalutae, invalidIndividual)
	    for indi, fit int zip(invalidIndividual, fitnesses):
        indi.fitness.values = fit

		# The population is entirely replaced by the offspring
        pop[:] = offspring
        
	return pop
```

`algorithms` 모듈사용 가능

            	
    	
## Installation
Python 2.7 / 3.4+ 가능

`SCOOP` 다중연산

`Numpy` CMA-ES

`matplotlib` 시각화

필요

**설치**

`easy_intsall deap`

or

`pip install deap`

or from source

`python setup.py intsall`
