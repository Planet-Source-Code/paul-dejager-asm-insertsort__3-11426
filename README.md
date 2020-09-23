<div align="center">

## Asm  InsertSort


</div>

### Description

This is a short snippit to use Intel assembler to do an insert sort.
 
### More Info
 
Have to use intel/windows machine. Linux uses a differnt assembler


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Paul DeJager](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/paul-dejager.md)
**Level**          |Beginner
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |C, Microsoft Visual C\+\+
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__3-1.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/paul-dejager-asm-insertsort__3-11426/archive/master.zip)





### Source Code

```
/*
Paul DeJager
A C/Assembler Insert Sort program
 This program uses assembler to do an insert sort routine for an array (a) of n size
 Here is the C code used for the insertsort logic
 for(j=1; j<n; j++)
{
     current = a[j];
     position = j;
     while(position > 0 && a[position-1] > current)
     	    {
 	        a[position] = a[position-1];
       	 	 position--;
     }
     a[position] = current;
}
 I declared and initialize your array and any other variables (such as n) in C code,
 And output the sorted array in C code. Everything in between is in assembly language
*/
#include <stdio.h>
#include <stdlib.h>
void printMe(int a[], int n); //print the array ... pretty simple
int main(void) {
	int a[25] = {12, 4, 20, 21, 13, 5, 19, 23, 3, 17, 16, 9, 7, 25, 6, 22, 2, 15, 8, 11, 24, 14, 1, 18, 10};
	int n = 25;
	int curr, p, j, pMinus;	//j outer loop var, p inner loop var, pMinus is p-1, curr is a[j]
							//or the current variable bieng evaluated
	__asm{
			mov eax, n		//we are going from a[0] to a[n-1] NOT to a[n]
			sub eax, 1		//dexcementing n by 1
			mov n, eax
			mov	ebx, 0		// ebx is the offset into the array, initialize to 0 for load
			mov j, ebx
			mov	ecx, n		// move n into loop counter (ecx)
	top:	//moving to the next array position and loading p and j
			mov eax, j
			add	eax, 4			// add 4 to offset to point at next int value
			mov j, eax
			mov p, eax
			mov	ebx, j
			mov	eax, a[ebx]		//loading curr to curr= a[j]
			mov curr, eax
	inner: mov eax, p			//loading p for cmp
			cmp eax, 0
			jle outbot			//less then = jmp to out of while
			mov eax, p			//p-1 offset
			sub eax, 4
			mov pMinus, eax
			mov ebx, pMinus		//a[p-1]
			mov eax, curr
			cmp a[ebx], eax		//cmp a[p-1] to curr
			jle outbot			//less then = jmp to out of while
			// a[p] = a[p-1];
			mov	ebx, pMinus		//a[p] = a[p-1];
			mov	edx, a[ebx]		//mov a[p-1] to edx
			mov ebx, p
			mov a[ebx], edx
			//p--
			mov eax, p		//mov pOff to eax
			sub eax, 4		//pOff -4
			mov p, eax		//storing eax into pOff
			jmp inner
	outbot: mov eax, curr
			mov ebx, p
			mov a[ebx], eax		//a[p] = curr;
			loop top			// ecx--, if > 0, branch to top
	//exit:	nop
	}
	printMe(a, n);
	return (0);
}
void printMe(int a[], int n){
 int i=0; int j = 1;
 printf("[");
 while(i < n)
	 printf(" %d", a[i++]);
 printf(" ]\n");
}
```

