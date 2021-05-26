---
title: "Data Structure_#4 순차적 자료구조(stack & Queue)"
categories:
  - Data Structure
  - Python
tags:
  - Data Structure
  - Python
---

# Stack

: 데이터 값을 저장하는 기본적인 구조로 일차원 선형(linear) 자료구조

LIFO( Last In First Out )

​	: 맨 마지막이 제일 먼저 나감

​							**<연산>**

​														push(a) : a 삽입  				->O(1)

​														pop(): 맨 위(뒤)값 삭제		->O(1)

​														top(): 맨 위(뒤)값 출력		->O(1)

​														isEmpty(): 비었는 지 확인 	->O(1)

​														size(): 크기							->O(1)

```python
class Stack:
    def __init__(self):			#special method
        self.items = []			#데이터 저장을 위한 리스트 준비(초기화)
        
    def push(self, val):
        self.items.append(val)
        
    def pop(self):
        try:
            return self.items.pop()		
        except IndexError:			#아이템이 비었을 경우
            print("Stack is empty")
    def top(self):
        try:
            return self.items[-1]
        except IndexError:
            print("Stack is empty")
            
    def __len__(self):				#Special method
        return len(self.items)		#len()로 호출시 stack의 item 수 반환
    
    def isEmpty(self):
        return len(self) == 0
```

#### Stack 활용 예시

##### 1) 괄호 맞추기

​	->입력: 오른쪽 왼쪽 괄호가 섞인 괄호 시퀀스

​	    출력: True(짝이 맞는 괄호일 경우) / Fasle(아닐 경우)

<Pseudo 코드>

```python
class Stack:
    def __init__(self):			#special method
        self.items = []			#데이터 저장을 위한 리스트 준비(초기화)
        
    def push(self, val):
        self.items.append(val)
        
    def pop(self):
        try:
            return self.items.pop()		
        except IndexError:			#아이템이 비었을 경우
            print("Stack is empty")
    def top(self):
        try:
            return self.items[-1]
        except IndexError:
            print("Stack is empty")
            
    def __len__(self):				#Special method
        return len(self.items)		#len()로 호출시 stack의 item 수 반환
    
    def isEmpty(self):
        return len(self) == 0

def parChecker(parSeq):
    S = Stack()
    for each symbol in parSeq:
        if symbol is "(":
            S.push(symbol)
        else:					#symbol == ")"
            if S is empty:		#if nothing matched
                return False
            else:				#저장된 ")"을 빼줌(짝 맞춰짐)
                S.pop()
    if S is empty:  			#stack에 남은 게 없는 경우(짝 다 맞춤)
        return True
    else:
        return False
```

```python
def parChecker(parSeq):
    S = Stack()
    for each symbol in parSeq:
        if symbol is "(":
            S.push(symbol)
        else:					#symbol == ")"
            if S is empty:		#if nothing matched
                return False
            else:				#저장된 ")"을 빼줌(짝 맞춰짐)
                S.pop()
    if S is empty:  			#stack에 남은 게 없는 경우(짝 다 맞춤)
        return True
    else:
        return False
```



###### 2) infix to postfix

​	**1. infix:** 일반적 수식 작성법 **ex) 1 + 2**

​	**2. prefix:** 연산자가 앞으로 **ex) + 2 3**

​	**3. postfix:** 연산자가 뒤로 **ex) 2 3 +**

