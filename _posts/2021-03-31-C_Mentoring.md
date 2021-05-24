---
title: "C Mentoring 1주차"
feature: /assets/img/C_Mentoring.png
tags:
  - HUFStory
  - C_Mentoring
---

# C Mentoring 1주차

## 조건문

if

## 반복문

for



while

## 과제

### 1. 조건문 개념 확인 문제

기준이 되는 삼각형의 높이와 밑변을 제공한다. (높이: 4, 밑변: 6)

사용자로부터 입력 받는 삼각형의 넓이가 주어진 삼각형의 넓이보다 클 때는 'bigger', 작을 때는 'smaller', 같을 때는 'same'을 출력하라.

```c
#include <stdio.h>

int main() {
//기준이 되는 삼각형의 높이와 밑변
    int crit_height = 4;
    int crit_base = 6;
    
//사용자로부터 입력받을 높이와 밑변 선언
    int height;
    int base;
    
    scanf("%d %d", &height, &base);
    
    int crit_area = crit_base * crit_height / 2;
    int area = base * height / 2;
    
//입력 받은 삼각형의 넓이와 기준이 되는 삼각형의 넓이 비교
    if(crit_area > area)
        printf("smaller\n");
    else if(cirt_area < area)
        printf("bigger\n");
    else
        printf("same\n");
    
    return 0;
}
```



### 2. 반복문 개념 확인 문제

사용자로부터 n을 입력 받고 n!(n 팩토리얼)을 출력해보자.

n!은 1부터 n이하의 값들을 모두 곱한 값을 의미한다.

```c
#include <stdio.h>

int main() {
//사용자로부터 n을 입력받는다
    int n;
    scanf("%d", &n);
    
//팩토리얼을 받을 변수 선언
    int result = 1;
    
//i를 1부터 n까지 result에 곱해준다
    for(int i = 1; i <= n; i++)
        result *= i;
    printf("n! = &d\n", result);
    
    return 0;
}
```

​	

### 3. 반복문 심화 문제

사용자로부터 숫자를 계속해서 입력 받다가, -1이 입력되면 현재까지 입력 받은 숫자들의 평균을 출력한다.

```c
#include <stdio.h>

int main() {
//사용자로부터 입력 받을 숫자를 저장하는 변수
    int num;
//반복문이 몇번 실행됐는지를 기록할 변수
    int count = 0;
//입력 받은 숫자들의 합을 저장하는 변수
    float sum = 0;
//숫자들의 평균을 저장하는 변수
    float aver;
    
//반복문이 몇번이나 반복할지 모르기 때문에 while(1)을 이용
    while(1) {
        scanf("%d", &num);
        
        if(num == -1) {//입력 받은 숫자가 -1일 때, 반복문 탈출
            aver = sum / count;
        }
        
        sum += num;
        count++;
    }
    
    printf("숫자들의 평균 : %.2f\n", aver);//소수점 둘째 자리까지 출력
    return 0;
}
```

