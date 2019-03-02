---
layout: post
title:  "GeeksforGeeks for C code(1)"
date:   2019-03-01 16:58:21 +0800
categories: C Program
---

> 嗨，大家好我是Jeremy，這是當初剛退伍在找工作的階段，準備面試時紀錄GeeksforGeeks上C語言的考題 part(1)，也歡迎大家多多交流，謝謝！

# **指標(進階)**


## Q1. 難度***

	void fun(int *p) {     
	       int q = 10; 
	       p = &q; 
	} 

	int main() {
	      int r = 20; 
	      int *p = &r; 
	      fun(p); 
	      printf("%d", *p); 
	      return 0; 
	}


**Sol**: 20, 由於fun傳遞並不是p的位置，在此處q只是pointer p的一份拷貝而已，當我們更改q去指向別的地方並不會更動到p，若要輸出正確為10，更動程式碼如下：

	void fun(int **ptr) { 

	      static int q = 10; 
	      *ptr = &q; 
	} 

	int main() {
	      int r = 20; 
	      int *p = &r; 
	      fun(&p); 
	      printf("%d", *p); 
	      return 0; 
	}

更改後，將p的位置傳入fun，fun的參數也要改為pointer的pointer，此時ptr的值也被改為q的位置。註：static變數為整個程式結束後才從記憶體中釋放。


## Q2. 難度**

Assume sizeof an integer and a pointer is 4 byte. Output?

	#include <stdio.h>
	#define R 10
	#define C 20
	int main(){
	     int (*p)[R][C];
	     printf("%d", sizeof(*p));
	     getchar();
	     return 0;
	}

**Sol**: 800, 10*20*sizeof(int) = 10*20*4 = 800，注意只針對把integer size視為4 byte的compiler。

## Q3. 難度**

	#include <stdio.h>

	int main(){
	     int a[5] = {1,2,3,4,5};
	     int *ptr = (int*)(&a+1);
	     printf("%d %d", *(a+1), *(ptr-1));
	     return 0;
	}

**Sol**: 2 5, 此處比較特別的是(&a+1)，&a表示size 5*integer_size的位置，再加1及指向第五個元素之後，所以減1之後就是指向第五個元素。


## Q4. 難度****

	#include <stdio.h>
	char *c[] = {"GeksQuiz", "MCQ", "TEST", "QUIZ"};
	char **cp[] = {c+3, c+2, c+1, c};
	char ***cpp = cp;
	int main(){
	     printf("%s ", **++cpp);
	     printf("%s ", *--*++cpp+3);
	     printf("%s ", *cpp[-2]+3);
	     printf("%s ", cpp[-1][-1]+1);
	     return 0;
	}

**Sol**: TEST, sQuiz, Z, CQ，此題相當的刺激，需要對指標和運算子的優先權有相當的了解，在此附上網址：運算子優先權。

第一項為 **(++cpp)，此時cpp指向c+2。 
第二項為 *(--*(++cpp))+3，cpp++指向c+1再取值為c+1這個位置，再減1(c+1-1)為c再取值為GeksQuiz，再加3為sQuiz，此時cpp指向c+1。 
第三項為 (*cpp[-2])+3, 因為cpp指向c+1，所以-2取出c+3的值(QUIZ)，再加3取出Z。 
第四項為 (cpp[-1][-1])+1。因為cpp指向c+1，所以第一個減一為指向c+2，再減一為(c+2-1)值為MCQ，加一印出CQ。


## Q5. 難度***

	#include <string.h>
	#include <stdio.h>
	#include <stdlib.h> 

	void fun(char** str_ref) {
	     str_ref++; 
	} 

	int main() { 

	     char *str = (void *)malloc(100*sizeof(char)); 
	     strcpy(str, "GeeksQuiz"); 
	     fun(&str); 
	     puts(str); 
	     free(str); 
	     return 0; 
	}

**Sol**: GeeksQuiz，因為str_ref只是fun的local變數，若要輸出eeksQuiz，fun裡則要改成(*str_ref)++。 