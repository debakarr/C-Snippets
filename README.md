#### This repository contains few code snippets for C language which I thing might worth knowing.

* Implementation of strlen

```C
int impStrlen (char *str) {

	// pointer to the starting address of str
	char *temp= str ;
	while (*temp++)
		// You can print value of temp to get an idea of what is happening
		//printf("%d\n", temp);
		;

	return (temp - str - 1 );
}
```

* Check if string is palindrome or not

```C
int palindromeCheck (char *str){
	char *p1 = str; // pointer pointing to starting of the string
	char *p2 = str + strlen (str) - 1; // pointer pointing to ending of the string

	while (p1 < p2){
		if (*p1++ != *p2--)
			return 0;		// not a palindrome
	}

	return 1;			// it is a palindrome
}
```

* Set or reset a specific bit in a given variable

e.g. 

8 -> 1000

we want to set 0th position so that it become 1001 (i.e. 9)

3 -> 0011

we want to reset 0th position so that it becomes 0010 (i.e. 2)

```C
#include <stdio.h>

// We can do a bitwise AND (&) or OR (|) to reset or set the bit
#define BITWISE_OP(x) (0x1 << x)

int set(int val, int pos){
    return (val |= BITWISE_OP(pos)) ;
}

int reset(int val, int pos) {
    return (val &= ~BITWISE_OP(pos)) ;
}

int main (){
  
    printf("%d\n", set(8,0));
    printf("%d\n", reset(3,0));

  return 0;
}
```

Output is:
9
2

* We can use bitwise operators in many situation

Rewriting **int c = a \* 16 + b / 2;**

```C
int c = (a << 4) + (b >> 1)
```

* Complement of integer without directly using **~**

refer to this: https://stackoverflow.com/questions/791328/how-does-the-bitwise-complement-operator-work

```C
#include <stdio.h>

void complement(int i) {
    printf( "Method 1: %d\n", i^(~0));
    printf( "Method 2: %d\n" , -(i+1)) ;
}

int main (){
    complement(5);
    return 0;
}
```

* Multiline macross

It is best practice to set a pointer to NULL after freeing it. We can use a macross for this. Below is the code for the same. 

```C
#include <stdio.h>
#include <stdlib.h>
#define freeNULL(p) {free(p); p = NULL; printf("FIN");}

int main (){
    
    int *ptr = (int *) malloc( 1 * sizeof(int *) );
    
    if(1)
        freeNULL(ptr);
    else
        printf("Nothing to do here...");
        
    return(0);
}
```

But this code gives an error message:

```
In function 'main':
12:5: error: 'else' without a previous 'if'
     else
     ^~~~
```

why so?

The reson is that after puting the macross the if statement becomes something like this:

```C
if(1){
	free(p); 
	p = NULL; printf("FIN");
};
```

The semicolon at the end cause the error. The Best way to overcome this is to use a do-while(0) loop. This loop runs only once, and after using this loop the if statement becomes something like this:

```C
if(1)
do {
	free(p); 
	p = NULL; printf("FIN");
}while(0);
```

So our program looks like:

```C
#include <stdio.h>
#include <stdlib.h>
#define freeNULL(p) do{free(p); p = NULL; printf("FIN");}while(0)

int main (){
    
    int *ptr = (int *) malloc( 1 * sizeof(int *) );
    
    if(1)
        freeNULL(ptr);
    else
        printf("Nothing to do here...");
        
    return(0);
}
```

