# Calculator
#include <stdio.h>
#include <stdlib.h>
#define MAX 100

typedef struct    //运算数
{
	int a[MAX];
	int top;
}OPND;

typedef struct    //运算符
{
	char a[MAX];
	int top;
}OPTR;

void Init_OPND(OPND *s); //初始化运算数栈 
void Push_OPND(OPND *s,int x); //push一个运算数 
int Pop_OPND(OPND *s); //pop一个运算数 
int GetTop_OPND(OPND *s); //取栈顶运算数 
void Init_OPTR(OPTR *s); //初始化运算符栈 
void Push_OPTR(OPTR *s,char x); //push一个运算符 
char Pop_OPTR(OPTR *s); //pop一个运算符 
char GetTop_OPTR(OPTR *s); //取栈顶运算符 
int IsOpr(char c); //判断输入字符是否为运算符 
char Precede(char s,char c); //判断字符的优先级 
int Operate(int x,char opr,int y); //计算

void Init_OPND(OPND *s)    //初始化运算数栈
{
	s->top =-1;
}


void Init_OPTR(OPTR *s)   //初始化运算符栈
{
	s->top =-1;
}

void Push_OPND(OPND *s,int x)   //push一个运算数
{
	s->top ++;
	s->a [s->top ]=x;
}

void Push_OPTR(OPTR *s,char x)   //push一个运算符
{
	s->top ++;
	s->a [s->top ]=x;
}

int Pop_OPND(OPND *s)     //pop一个运算数
{
	int x;
	x=s->a [s->top];
	s->top --;
	return x;
}

char Pop_OPTR(OPTR *s)    //pop一个运算符
{
	char x;
	x=s->a [s->top];
	s->top --;
	return x;
}

int GetTop_OPND(OPND *s) //取栈顶运算数
{
	return (s->a[s->top]);
}

char GetTop_OPTR(OPTR *s)   //取栈顶运算符
{
	return (s->a[s->top]);
}

int IsOpr(char c)   //判断输入字符是否为运算符 
{
	if (c=='+'||c=='-'||c=='*'||c=='/'||c=='('||c==')'||c=='#')
		return 1;
	else
    	return 0;
}

char Precede(char s,char c) //判断字符的优先级 
{
	switch(s)
	{
		case '+':
		case '-':			
			{
				if(c=='+'||c=='-')
    			return '>';
   				else if (c=='*'||c=='/')
      			return '<';
   				else if(c=='(') 
    			return '<'; 
   				else if(c==')') 
    			return '>'; 
   				else 
    			return '>';
			}
			break;

		case '*':
		case '/':
			{
				if(c=='+'||c=='-')
    			return '>';
   				else if (c=='*'||c=='/')
    			return '>';
   				else if(c=='(') 
    			return '<'; 
   				else if(c==')') 
    			return '>'; 
   				else 
				return '>'; 
			}
			break;

		case '(':
			{
				if(c==')')
    			return '=';
   				else 
    			return '<';
			}
			break;

		case ')':
			{
   				return '>';
			}
			break;

		case '#':
		{
			if(c=='#')
     		return '=';
    		else
    		return '<';
		}
		break;
	}
}

int Operate(int x,char opr,int y)   //计算
{
	int result; 
	switch (opr) 
	{ 
		case '+': 
     		result = x + y; 
     		break; 
		case '-': 
   			result = x - y; 
   			break;
    	case '*': 
   			result = x * y; 
   			break;
    	case '/': 
   			result = x / y; 
   			break;
	}
	return result;
}


int main()
{
	char c;
	OPND sdata; 
	OPTR soper;
	int a,b,result,i;
	char ch,theta;
	Init_OPND(&sdata);
	Init_OPTR(&soper);
	Push_OPTR(&soper,'#');
	do
	{ 
	printf("是否运行，若进行请输入Y/y\n");
	fflush(stdin);
	scanf("%c",&c);
	printf("请输入运算式，以#号结尾\n");
	ch=getchar();
	while(ch!='#'||GetTop_OPTR(&soper)!='#') //当读入的字符和OPTR栈顶的字符均为‘#’时结束运算
	{
		if(!IsOpr(ch))                           //是运算数的情况
		{
			i=atoi(&ch);                           //将字符型转为整型
			ch=getchar();                           //使得可以输入几位数
   			while(!IsOpr(ch))          
   			{
    			i=i*10+atoi(&ch);
    			ch=getchar();
   			}
   			Push_OPND(&sdata,i); 
		}
		else
		{
   			switch(Precede(GetTop_OPTR(&soper),ch))    //比较栈顶运算符和刚输入运算符的优先级
   			{    
   				case '<':
    			Push_OPTR(&soper,ch);
    			ch=getchar();
    			break;
    			
   				case '=':
    			theta=Pop_OPTR(&soper);
    			ch=getchar();
    			break;
   			
				case '>':
    			theta=Pop_OPTR(&soper);
    			b=Pop_OPND(&sdata);
    			a=Pop_OPND(&sdata);
    			result=Operate(a,theta,b);
    			Push_OPND(&sdata,result);
    			break;
   			}
		}
	} 
	printf("%d\n",GetTop_OPND(&sdata));
    }while(c=='y'&&'Y');
	return 0;
}
