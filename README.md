# Scientific-calculator-using-c-languge
## what are the operation performed?
    This calculator will perform arithmatic operations (Addition,Substraction,Multiplication,Division) along with some trignometric functions sin,cos,tan and some other functions like squire root,log,power,exponential.
## What is the input format?
     it will ask for entering the expression. we can input the expression using brackets also for example we can give input like (20+40) or 20+40 also.
    
    ENTER THE EXPRESSION
    20+40
    
    after entering the expression it will show like this
    
    ENTER THE EXPRESSION
    20+40
    CURRENT MODES
    ---------------------------
    |0|FIX   |1|RAD(1)/DEGREE(0)
    ---------------------------
    SELECT MODE
    1->FIX
    2->RAD
    3->DEGREE
    4->EVALUVATE
    
    Now we have to select among 4 option
    1.if we want to fix the digits of decimals in output.
       if we select 1 than it will ask for 
       ENTER THE EXPRESSION
        20+40
        CURRENT MODES
        ---------------------------
        |0|FIX   |1|RAD(1)/DEGREE(0)
        ---------------------------
        SELECT MODE
        1->FIX
        2->RAD
        3->DEGREE
        4->EVALUVATE
        1
        ENTER THE NUMBER OF DECIMALS 0-6
        
    we have to enter the required fix in the range of 0-6
    for example fixing for 4 digit than
        ENTER THE EXPRESSION
        20+40
        CURRENT MODES
        ---------------------------
        |0|FIX   |1|RAD(1)/DEGREE(0)
        ---------------------------
        SELECT MODE
        1->FIX
        2->RAD
        3->DEGREE
        4->EVALUVATE
        1
        ENTER THE NUMBER OF DECIMALS 0-6
        4
       After entering the fix mode is activated for 4 digits.
    
    2.if we want to evaluvate the expression in radian mode.
       initially calculator is in rad mode.
    3.if we want to evaluvate the expression in degree mode.
    4.for evaluvating the expression.
    
    
    #Code:
    #include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<math.h>
#define MAX 1000
char fix='0',rad='1';
int dec=3;
struct stack
{
    double data[MAX];
    int top;
};
typedef struct stack stack;
struct cstack
{
    char data[MAX];
    int top;
};
typedef struct cstack cstack;

static FILE *input,*output;

void error_shower(int error_code)
{
    switch(error_code)
    {
        case 1:printf("Math error");
               exit(0);
        case 2:printf("Syntax error");
               exit(0);
    }
}

int isfull_c(cstack p)
{
    if(p.top==MAX-1)
        return 1;
    else
        return 0;
}
int isempty_c(cstack p)
{
    if(p.top==-1)
        return 1;
    else
        return 0;
}
void push_c(cstack *p,char c)
{
    if(isfull_c(*p))
    {
        error_shower(2);
    }
    else
    {
        p->top++;
        p->data[p->top]=c;
    }
}
void pop_c(cstack *p)
{
    int temp;
    if(isempty_c(*p))
    {
       error_shower(2);
    }
    else
    {
        p->top--;
    }
}
char peek_c(cstack *p)
{
    return p->data[p->top];
}
int isfull(stack p)
{
    if(p.top==MAX-1)
        return 1;
    else
        return 0;
}
int isempty(stack p)
{
    if(p.top==-1)
        return 1;
    else
        return 0;
}

void push(stack *p,double num)
{
    if(isfull(*p))
    {
        error_shower(2);
    }
    else
    {
        p->top++;
        p->data[p->top]=num;
    }
}
void pop(stack *p)
{
    int temp;
    if(isempty(*p))
    {
      error_shower(2);
    }
    else
    {
        p->top--;
    }
}
double peek(stack *p)
{
    return p->data[p->top];
}

int priority(char ch)
{
    switch (ch)
    {
    	case '(':
    		return 1;
	    case '+':
	    case '-':
	        return 2;

	    case '*':
	    case '/':
	        return 3;

	    case '^':
	        return 4;
        case 's':
        case 'c':
        case 't':
        case 'l':
        case 'e':
            return 5;
    }
    return -1;
}

double calculate(char op, double l , double r)
{
	if(op == '+'){
		return l + r;
	}else if(op == '-'){
		return l - r ;
	}else if(op == '*'){
		return l * r;
	}else if(op == '/'){
		if(r != 0){
			return (double)l/r;
		}
		else
        {
            error_shower(1);
        }

	}else if(op == '^'){
		return pow(l,r);
	}
	return -1;
}


double number_maker(char str[100])
{
 int point=-1;
 double a=0,b=0;
 for(int i=0;i<strlen(str);i++)
 {
     if(str[i]=='.')
     {
         point=i;
         break;
     }
 }
 if(point==-1)
     {
         point=strlen(str);
     }
     int k=0;
 for(int i=point-1;i>=0;i--)
         {
             a=a+((int)(str[i])-48)*pow(10,k);
             k++;
         }
         k=-1;
         for(int i=point+1;i<strlen(str);i++)
         {
              b=b+((int)(str[i])-48)*pow(10,k);
             k--;
         }
         return a+b;
}
double trignometric_eval(char c,double val)
{
    double value;
    if(rad=='0')
    {
        value=val*M_PI/180;
    }
    else
        value=val;
    switch(c)
    {
        case 's':return sin(value);
                 break;
        case 'c':return cos(value);
                 break;
        case 't':return tan(value);
                 break;
        case 'i':return log(val);
                 break;
        case 'e':return exp(val);
                 break;
        case 'x':if(val<0)
                  error_shower(1);
                 else
                  return sqrt(val);
                 break;
    }
}
void mode_shower()
{
    int choice;
    while(choice!=4)
    {
    printf("CURRENT MODES\n");
    printf("---------------------------\n");
    printf("|%c|FIX   |%c|RAD(1)/DEGREE(0) \n",fix,rad);
    printf("---------------------------\n");
    printf("SELECT MODE\n1->FIX\n2->RAD\n3->DEGREE\n4->EVALUVATE\n");
        scanf("%d",&choice);
        switch(choice)
    {
        case 1:printf("ENTER THE NUMBER OF DECIMALS 0-6\n");
               scanf("%d",&dec);
               fix='1';
               break;
        case 2:rad='1';
               break;
        case 3:rad='0';
               break;
        case 4:return;
        default:printf("INVALID CHOICE\n");
    }
    system("cls");
    }
}
int main()
{
    stack operand;
    cstack op_r;
    operand.top=-1;
    op_r.top=-1;

    char str[1000] ;
    printf("ENTER THE EXPRESSION\n");
    scanf("%s",str);
    mode_shower();
	double l = sizeof(str)/sizeof(char);
	int k = 0;
	int i = 0;
	while(str[i] != '\0')
        {
		if(str[i] == '(')
        {
			push_c(&op_r,'(');
		}
        else if(str[i] == ')')
            {
			while(peek_c(&op_r) != '(')
            {
                char c=peek_c(&op_r);
                if(c=='s'||c=='c'||c=='t'||c=='l'||c=='e'||c=='x')
                {
                    double val=peek(&operand);
                    pop(&operand);
                    double re=trignometric_eval(c,val);
                    push(&operand,re);
                    pop_c(&op_r);
                }
				else
                {
                    double r =peek(&operand);
				pop(&operand);
				double l = peek(&operand);
				pop(&operand);
				double re = calculate(peek_c(&op_r),l,r);
				push(&operand,re);
			  pop_c(&op_r);
                }
			}
			pop_c(&op_r);;
		}
		else if(str[i] == '+' || str[i] == '-' || str[i] == '*' || str[i] == '/' || str[i] == '^'||str[i]=='s'||str[i]=='c'||str[i]=='t'||str[i]=='l'||str[i]=='e')
		{
			int pC = priority(str[i]);
			if(i==0&&str[i]=='-'||i==0&&str[i]=='-'&&str[i+1]=='(')
            {
                if(str[i+1]=='(')
                    {
                        push_c(&op_r,'(');
                        i+=2;
                    }
                else
                    i++;
                char st1[1000]="";
                char st2[2]="";
                for(i;i<strlen(str);i++)
                {
                    if((int)str[i]>47&&(int)str[i]<58)
                    {
                        st2[0]=str[i];
                        strcat(st1,st2);
                    }
                    else
                        break;
                }
                double val=number_maker(st1);
                val=-val;
                push(&operand,val);
                i--;
            }
            else if(str[i]=='-'&&str[i-1]=='(')
            {
                i++;
                char st1[1000]="";
                char st2[2]="";
                for(i;i<strlen(str);i++)
                {
                    if((int)str[i]>47&&(int)str[i]<58)
                    {
                        st2[0]=str[i];
                        strcat(st1,st2);
                    }
                    else
                        break;
                }
                double val=number_maker(st1);
                val=-val;
                push(&operand,val);
                i--;
            }
            else
			if(str[i]=='s'||str[i]=='c'||str[i]=='t'||str[i]=='l'||str[i]=='e')
            {
            if(str[i]=='s'&&str[i+1]=='q'&&str[i+2]=='r'&&str[i+3]=='t')
            {
                push_c(&op_r,'x');
                i+=3;
            }
            else if(str[i]=='s'&&str[i+1]=='i'&&str[i+2]=='n')
            {
                push_c(&op_r,str[i]);
                i+=2;
            }
            else if(str[i]=='c'&&str[i+1]=='o'&&str[i+2]=='s')
            {
                push_c(&op_r,str[i]);
                i+=2;
            }
            else if(str[i]=='t'&&str[i+1]=='a'&&str[i+2]=='n')
            {
                push_c(&op_r,str[i]);
                i+=2;
            }
            else if(str[i]=='l'&&str[i+1]=='o'&&str[i+2]=='g')
            {
                push_c(&op_r,str[i]);
                i+=2;
            }
            else if(str[i]=='e'&&str[i+1]=='x'&&str[i+2]=='p')
            {
                push_c(&op_r,str[i]);
                i+=2;
            }
            else
            {
                error_shower(3);
            }
          }
			else
			{
			    while(!isempty_c(op_r) && priority(peek_c(&op_r)) >= pC)
                {
                char c=peek_c(&op_r);
                if(c=='s'||c=='c'||c=='t'||c=='l'||c=='e'||c=='x')
                {
                    double val=peek(&operand);
                    pop(&operand);
                    double re=trignometric_eval(c,val);
                    push(&operand,re);
                    pop_c(&op_r);
                }
				else
                {
                    double r = peek(&operand);
				pop(&operand);
				double l =  peek(&operand);
				pop(&operand);
				double re = calculate(peek_c(&op_r),l,r);
				push(&operand,re);
				 pop_c(&op_r);
                }
			    }
			    push_c(&op_r,str[i]);
			}

		}
		else
		{
            char num[100]="";
            char numcat[2]="";
            for(i;i<strlen(str);i++)
              {
                 if(((int)str[i]>47 &&(int)str[i]<58)||str[i]=='.')
                 {
                     numcat[0]=str[i];
                     strcat(num,numcat);
                 }
                 else
                    break;
              }
                  double val=number_maker(num);
                  //printf("%lf",val);
                  push(&operand,val);
                  i--;
		}
        i++;
	}
	while(!isempty_c(op_r))
        {
        char c=peek_c(&op_r);
        if(c=='s'||c=='c'||c=='t'||c=='l'||c=='e'||c=='x')
                {
                    double val=peek(&operand);
                    pop(&operand);
                    double re=trignometric_eval(c,val);
                    push(&operand,re);
                    pop_c(&op_r);
                }
		else
		{
        double r =  peek(&operand);
		pop(&operand);
		double l = peek(&operand);
		pop(&operand);
		double re = calculate(peek_c(&op_r),l,r);
		push(&operand,re);
        pop_c(&op_r);
		}
	}
	printf("\n\nANSWER:\n");
     if(fix=='1')
     {
         switch(dec)
         {
             case 1:printf("%.1lf",peek(&operand));
                    break;
             case 2:printf("%.2lf",peek(&operand));
                    break;
             case 3:printf("%.3lf",peek(&operand));
                    break;
             case 4:printf("%.4lf",peek(&operand));
                    break;
             case 5:printf("%.5lf",peek(&operand));
                    break;
             case 6:printf("%lf",peek(&operand));
                    break;
             case 0:printf("%.lf",peek(&operand));
                    break;
         }
     }
     else
         printf("%lf",peek(&operand));



    return 0;
}


       
