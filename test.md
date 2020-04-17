## 位运算
1000个学生一个班最多分32个人，最少可以分多少班。
1000/32，得到的商+1
- 1000转化成二进制数 
- 32是2的5次方
- 二进制数右移5位就得到商。+1即可


## n条直线划分的最多空间l(n)=n(n+2)/2+1
```
//n条直线划分最多空间l(n)=n(n+2)/2+1
##include <stdio.h>
int calc_spaces(int n); // n >= 0

int main(void)
{
    int n;
    printf("输入n的值:\n");
    scanf("%d",&n);
    printf("%d条直线最多划分%d个空间\n",n,calc_spaces(n));
}

int calc_spaces(int n)
{
    return (n*(n+2))/2+1;
}
```
程序执行结果：
```
输入n的值:
3
3条直线最多划分8个空间
Program ended with exit code: 0
```

## 打印三角形
```
/*
 打印三角形
       1
     2   2
   3   4   3
 4   7   7   4
*/
#include <stdio.h>
#define N 10
void draw(unsigned int n)
{
    int i,j,k;
    int a[N][N];
    for (i=1;i<=n;i++)
        a[i][1] = a[i][i] = i;//两边的值为i
    for(i=3;i<=n;i++)
        for(j=2;j<=i-1;j++)
            a[i][j]=a[i-1][j-1]+a[i-1][j];  //除两边的数外的其他数都等于上层两数和
    for(i=1;i<=n;i++)
    {
        for(k=1;k<=n-i;k++)
            printf("   ");
        for(j=1;j<=i;j++)  //j<=i输出想要的数
            printf("%6d",a[i][j]);
        printf("\n");
        
    }
}
int main(void)
{
    unsigned int n;
    printf("输入n的值：");
    scanf("%d",&n);
    draw(n);
    printf("\n");
}

```
执行程序结果
```
输入n的值：5
                 1
              2     2
           3     4     3
        4     7     7     4
     5    11    14    11     5

Program ended with exit code: 0
```

## 实现atof函数
- 函数定义：
>>double my_atof(char *nptr);
-实现功能：
my_atof() 会扫描参数nptr字符串，跳过前面的空格字符，直到遇上数字或 . 符号才开始做转换，而再遇到非数字或字符串结束时('\0')才结束转换，并将结果返回。

以下都是合法输入:
```
0.123
.123
16.4
16.
0.0
0.

注意: 不考虑 +- 符号, 不考虑输入非法的情况
```

```
#include <stdio.h>
double my_atof(char *nptr)
{
    double nums=0;
    double temp = 10;
    while (*nptr ==' ')
    {
        nptr++;
    }
    while (*nptr>='0'&&*nptr<='9'&&*nptr!='.')
    {
       
        nums = nums * 10 + (*nptr - '0');
        nptr++;
    }
    if(*nptr == '.')
        nptr++;
    while(*nptr>='0'&&*nptr<='9')
    {
        nums = nums + (*nptr-'0')/temp;
        temp = temp*10;
        nptr++;
    }
    return nums;
}
int main(void)
{
    char *nptr = "123.4";
    printf("%lf\n",my_atof(nptr));
}
```
执行结果：
```
123.400000
Program ended with exit code: 0
```

## 使用栈的数据结构实现队列
用两个栈实现队列的数据操作。
因为栈是先进后出，队列是先进先出，所以需要两个栈倒数据才能达到队列的数据操作要求，出队操作时，a1,a2,a3按顺序进入in栈，再按照a3,a2,a1顺序出栈然后进入out栈再正常出栈即可。
入队操作时，a1,a2,a3按照顺序进入in栈，然后按照a3,a2,a1出栈进入out栈，再正常出栈，顺序a1,a2,a3


```
//使用栈的数据结构实现队列
//栈先进后出，队列先进先出，
//stack：in->a1->a2-a3->out->stack2->in->a3->a2->a1->out
//queue:in:a1->a2->a3 ;out :a1->a2->a3
#include <stdio.h>
#include <limits.h>
#include <stdlib.h>

typedef struct stack{
    int elem;
    struct stack *next;
} Stack, Queue;
typedef struct Queue{
    Stack stack1;
    Stack stack2;
}sQueue;

Stack* init_stack(void);
void free_stack(Stack* head);
int push(Stack* head, int elem);
int pop(Stack* head);
int top(Stack* head);
int is_full(Stack* head);
int is_empty(Stack* head);

Stack* init_stack(void) {
    Stack* head = malloc(sizeof(Stack));
    head->next = NULL;
    return head;
}


void free_stack(Stack* head) {
    Stack* t;
    while(head->next != NULL) {
        t = head->next;
        free(head);
        head = t;
    }
    free(head);
}

int push(Stack* head, int elem) {
    Stack *new_node = malloc(sizeof(Stack));
    if (new_node == NULL) {
        // 分配内存失败
        return 0;
    }
    new_node->elem = elem;
    new_node->next = head->next;
    head->next = new_node;
    return 1;
}

int pop(Stack* head) {
    if (is_empty(head)) {
        printf("stack is empty\n");
        return INT_MIN;
    }
    Stack* t = head->next;
    head->next = t->next;
    int elem = t->elem;
    free(t);
    return elem;
}

int top(Stack* head) {
    if (is_empty(head)) {
        printf("stack is empty\n");
        return INT_MIN;
    }
    return head->next->elem;
}

int is_empty(Stack* head) {
    return (head->next == NULL)?1:0;
}
void enqueue(sQueue* queue, int data)
{
    Stack *s1,*s2;
    s1 = &(queue->stack1);
    s2 = &(queue->stack2);
    push(queue, data);
}// 函数类型请自己考虑
int dequeue(sQueue* queue)
{
    Stack *p1, *p2;
    p1 = &(queue->stack1);
    p2 = &(queue->stack2);
    int data;
  
    if (is_empty(p2)){
        while (!is_empty(p1)){
            data = top(p1);
            pop(p1);
            push(p2,data);
        }
    }
    return pop(p2);
}
int main(void)
{
    Queue* queue = init_stack();
    int a[5] = {1, 2, 3, 4, 5};
    for( int i = 0; i < 5; i++) {
        enqueue(queue, a[i]);
    }
    
    for (int i = 0; i < 5; i++) {
        int out = dequeue(queue);
        printf("%3d",out);
    }
    printf("\n");
    return 0;
}

```