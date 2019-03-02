---
layout: post
title:  "GeeksforGeeks for C code(2)"
date:   2019-03-02 13:00:21 +0800
categories: C Program
---

> 嗨，大家好我是Jeremy，這是當初剛退伍在找工作的階段，準備面試時紀錄GeeksforGeeks上C語言的考題 part(2)，也歡迎大家多多交流，謝謝！

# **指標(進階)**


## Q1. 難度** 

Assume that the size of int is 4.

	#include <stdio.h>
	void f(char**);
	int main(){

	      char *argv[] = { "ab", "cd", "ef", "gh", "ij", "kl" };
	      f(argv);
	      return 0;
	}

	void f(char **p){
	     char *t;
	     t = (p += sizeof(int))[-1];
	     printf("%s\n", t);
	}

**Sol**: gh, (p += sizeof(int))[-1] ⇨ (p+=4)[-1] ⇨ (p = p+4-1) ⇨ (p = p+3)

## Q2. 難度***
	#include <stdio.h>
	int main(){
	     int a[][3] = {1, 2, 3, 4, 5, 6};
	     int (*ptr)[3] = a;
	     printf("%d %d ", (*ptr)[1], (*ptr)[2]);
	     ++ptr;
	     printf("%d %d\n", (*ptr)[1], (*ptr)[2]);
	     return 0;
	}

**Sol**: 2,3,5,6 觀念：(*ptr)[3] = 一個a[][3]的二維陣列，這樣就很好解了，++ptr為指向第二個row及指向4，所以(*ptr)[1]為5 (*ptr)[2]為6。

註：(*ptr +5)[3]表示從第一個元素p[0][0]開始，往後5個位置當新起點，再從新起始點往後算3個位置。 (*(ptr +5))[3]表示從p[0][0]往下數5個row(ptr[5][0])，再往下數3個位置。


## Q3. 難度***
	#include <stdio.h>
	#include <stdlib.h>

	int main(void){
	     int i;
	     int *ptr = (int *) malloc(5 * sizeof(int));

	     for (i=0; i<5; i++)
	          *(ptr + i) = i;

	     printf("%d ", *ptr++);
	     printf("%d ", (*ptr)++);
	     printf("%d ", *ptr);
	     printf("%d ", *++ptr);
	     printf("%d ", ++*ptr);
	}

**Sol**: 0,1, 2, 2,3 *ptr++ = *(ptr++)，先做++後序式的部分再取值，但是後序式會先回傳值，故答案為0，並且ptr指向下一個1， (*ptr)++會先回傳*ptr = 1再把1加1等於2，故答案會是1，第三項*ptr = 2因為上式已把1加上1並回存到，此時ptr還是指向第二個元素，*++ptr = *(++ptr)，ptr指向第三個元素故為2，++*ptr = ++(*ptr)取出2後加1，此時陣列最終為0,2,3,3,4。


## Q4. 難度*
Output of following program

	#include <stdio.h>
	int fun(int arr[]) {
	     arr = arr+1; 
	     printf("%d ", arr[0]);
	}

	int main(void) {
	     int arr[2] = {10, 20};
	     fun(arr);
	     printf("%d", arr[0]);
	     return 0;
	}

**Sol**: 20, 10，當arr傳遞進fun裡時會被當成指標來看，所以當arr = arr+1時會指向20所以arr[0]會印出20，main裡的arr[0]依舊指向10。


## Q5. 難度**
What is the output of the following C code? Assume that the address of x is 2000 (in decimal) and an integer requires four bytes of memory.

	#include <stdio.h> 
	int main() { 
	     unsigned int x[4][3] = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9}, {10, 11, 12} }; 
	     printf("%u, %u, %u", x+3, *(x+3), *(x+2)+3); 
	}

**Sol**: 2036,2036, 2036， 因為x+3指向的是第四列，所以值為2000+ 3(列)*3 (個數 )*4(bytes) = 2036，*(x+3)一樣印出第四列的位置， *(x+2)+3 = 指向第四列的第一的數 = 2000 + 2*3*4 + 3*4 = 2036。


## Q6. 難度***

	# include <stdio.h>
	int main( ) { 
	     static int a[] = {10, 20, 30, 40, 50};
	     static int *p[] = {a, a+3, a+4, a+1, a+2};
	     int **ptr = p;
	     ptr++;
	     printf("%d%d", ptr - p, **ptr);
	}

**Sol**: 140, p為一陣列，而每個元素又有pointer指向他們，故裡面存的是a裡元素的位置，用以下來表示(令1000為初始位置)：
Array a:          10           20           30            40            50 
address:        1000       1004        1008        1012       1016

Array p:       1000        1012        1016        1004       1008 
address:        2000       2004        2008        2012       2016

所以ptr++為ptr指向下一位置(a+3)，所以ptr-p = 1，**ptr = 取a+3的值= 40。