---
layout: post
title: "MIPS Assembly Example"
---

MIPS

---
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