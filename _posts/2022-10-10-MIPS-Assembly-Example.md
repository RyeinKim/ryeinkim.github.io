---
layout: post
title: "MIPS Assembly Example"
---

컴퓨터 구조를 배우면서 MIPS Assembly 에 대해 배웠고, 과제를 받았다.
<br>과제는 MIPS를 사용해서 _**<u>0부터 입력한 수까지 짝수의 곱</u>**_ 을 구하는 과제였다.
<br>주석을 포함할 경우 가산점이 높았기 때문에 주석을 포함하여 제출하였다.

<br>

> final.s

```bash
#프로그램에서 사용할 데이터 영역
.data
msg: .asciiz "Calculation result: " #출력할 메세지
number: .word 20 #연산에 사용할 수

#프로그램의 코드영역
.text
.globl main

main:
    lw $s0, number #s0레지스터에 20을 가지고 온다.
    li $t0, 4 # 4로 나눗셈 연산을 하기위한 t0 레지스터
    li $s1, 0 # 반복문 조건용 s1 레지스터 (s1이 21이 되면 반복문 종료)
    li $t4, 1 # 답이 곱셈이기 때문에 t4 레지스터에 1을 넣음


loop:  
    addi $s1, $s1, 1 # s1 레지스터 값을 1 올려줌
    beq $s1, 21, exit # 만약 s1 레지스터가 21과 같다면 종료한다
    div $s1, $t0 # $s1/$t0
    mfhi $t2 # 나눗셈 결과의 나머지를 t2 레지스터에 저장
    bne $t2, $zero, loop # t2 레지스터가 0과 같지 않다면 반복문 break
    mul $t4, $t4, $s1 # t4 레지스터와 s1 레지스터를 곱셈
    j loop # 다시 돌림

exit:

#결과 출력 부분
#msg로 레이블 된 메세지를 출력
li $v0, 4
la $a0, msg
syscall

#계산 값을 출력하는 부분
#최종 결과는 $t4레지스터에 저장되어 있다고 가정하고 출력
li $v0, 1
addi $a0, $t4, 0
syscall

#프로그램 종료
li $v0, 10  # exit
syscall
```
지금보면 주석을 깔끔하게 사용한 것 같지는 않다.
<br>주석을 명령어 하나하나 옆에 작성해주는 것보다 이 프로그램이 어떻게 동작하는지의 큰 그림과
코드 내부에서 꼭 알려줘야 할 정보에 대해서만 주석으로 작성하는 습관을 갖는게 더 좋을 것 같다.