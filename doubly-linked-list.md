# 双向链表（Doubly Linked list）+循环链表约瑟夫环思路+使用栈的数据结构实现后缀表达式的计算

### Doubly Linked list

- 结构体实现链表的每个结点

```
typedef struct node 
{
    struct node *pre;
    struct node *next;
    int elem;
} Node;
```

每创建一个新的结点，都要与前驱结点建立两次联系

- 新结点的pre指针指向前驱结点
- 指向前驱结点的next指向新结点

**创建一个新结点**

```
*创建一个结点*/
Node *initNode(Node *prenode, int elem) {
    Node *node = (Node *)malloc(sizeof(Node));//新结点申请内存
    node->pre = NULL; //初始化的当前结点的pre指针指向NULL
    node->next = NULL;//初始化的当前结点的next指针也指向NULL
    node->elem = elem;
    
    //和前结点建立双层逻辑
    node->pre = prenode;//当前结点的pre指针指向前结点
    prenode->next = node;//前结点的next指针指向当前结点
    
    return node;
}
```

**display函数直观打印链表**
```
/*显示链表*/
void display(Node *p) {
    Node *temp = p;
    printf("共%d个元素：", length);
   
    while (temp->next) {
        temp = temp->next;
        printf("  %d  ", temp->elem);
        
        if (temp->next) {
            printf("<-->");
        }
    }
    
    printf("\n");
}
```
**先要找到指定位置的上一个结点**
```
/*找到指定位置的上一个节点*/
Node *getPreNode(Node *head, int pos, int min , int max) {
    if (pos > max   || pos < min) {
        printf("位置有误\n");
        return NULL;
    }
    
    Node *temp = head;
    
    for (int i = 0; i < pos; i++) {
        temp = temp->next;
    }
    
    return temp;
}

```

**增操作**

```
/*增*/
void add(Node *head, int elem, int pos) {
    Node *pre = getPreNode(head, pos, 0, length);
    
    if (pre == NULL) {
        return;
    }
    
    Node *next = pre->next;//需要插入位置的后结点
    
    //创建一个新结点
    Node *add = (Node *)malloc(sizeof(Node));
    add->elem = elem;
    
    //和前结点建立双层逻辑
    add->pre = pre;
    pre->next = add;
    
    if (next != NULL) {
        //和后结点建立双层逻辑
        add->next = next;
        next->pre = add;
    } else {
        add->next = NULL;
    }
    
    length ++;//表长度+1
}

```

**删除操作**

```
/*删*/
void delete(Node *head, int pos) {
    Node *pre = getPreNode(head, pos, 0, length - 1);
    
    if (pre == NULL) {
        return;
    }
    
    Node *del = pre->next;//需要删除的结点
    Node *next = del->next;//需要删除结点的后结点
    
    pre->next = next;//将前结点的next指针指向后结点
    
    if (next != NULL) {
        next->pre = pre;    //将后结点的pre指针指向前结点
    }
    
    free(del);//释放删除结点空间
    del = NULL;
    length --;//表长度-1
}

```

**主函数**
```
int main() {
    Node *head = (Node *)malloc(sizeof(Node));//创建头结点
    Node *pre = head;//将头结点作为首元结点的前一个结点
    
    for (int i = 0; i < SIZE; i++) {
        pre = initNode(pre, i + 1);
    }
    
    length = SIZE;
    
    display(head);
    
    printf("在第%d个位置上插入元素%d。\n", ADDPOS, ADDNUM);
    add(head, ADDNUM, ADDPOS);
    display(head);
    
    printf("删除第%d个位置上元素\n", DELPOS);
    delete(head, DELPOS);
    display(head);
    
    return 0;
}


```

**执行结果**

```
共5个元素：  1  <-->  2  <-->  3  <-->  4  <-->  5  
在第5个位置上插入元素9。
共6个元素：  1  <-->  2  <-->  3  <-->  4  <-->  5  <-->  9  
删除第5个位置上元素
共5个元素：  1  <-->  2  <-->  3  <-->  4  <-->  5  
Program ended with exit code: 0

```


## 约瑟夫环循环链表解决思路

**约瑟夫环问题是一个数学的应用问题：已知n个人(以编号1,2,3...n分别表示)围坐在一张圆桌周围。从编号为k的人开始报数，数到m的那个人出列，他的下一个人又开始报数，数到m的那个人又出列，依次规律重复下去，直到只剩最后一人。**

- n个人围成环状
- 从k个编号的人开始报数
- 数到m个人的时候此人出列（k+m），下一个人报数
- 然后一直循环，直到n-1人全部出列，最后一人胜出。

首先定义一个结构体

```
typedef struct node
{
    struct node *next;
    int elem;
}Node;

```
while（node->next !=node）
{
    从一个k结点开始遍历，间隔m个删除一个结点；
    然后在间隔m删除一个结点
    直到这个结点的next:node->next=node;

}
可以跳出约瑟夫环

## 使用栈的数据结构实现后缀表达式的计算
**5 1 2 + 4 * + 3 -**

- 初始化栈，栈顶指针为空
- 遇到操作数5，入栈
- 遇到操作数1，再入栈5，1
- 遇到操作数2，再入栈5，1，2
- 遇到加法操作符，1 2，出栈，结果3入栈，5，3
- 遇到操作数4，入栈，5，3，4
- 遇到乘法运算符，3，4出栈，结果12入栈，5，12
- 遇到加法运算符，5，12出栈，结果17入栈，
- 操作数3入栈，17，3
- 遇到减法运算17，3出栈，结果14入栈
```

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100
typedef int ElemType;
typedef struct node {
    ElemType date;
    struct node *next;
} StackNode;

void InitStack(StackNode **p) {//栈的声明
    (*p)=NULL;
}

int StackEmpty(StackNode *p) {//如果栈是空的返回0；不空返回1
    if(p==NULL)
        return 0;
    return 1;
}

void Push(StackNode **top, ElemType x) {//入栈操作
    StackNode *p;
    p=(StackNode *)malloc(sizeof(StackNode));
    p->date=x;
    p->next=*top;
    *top=p;
}

void Pop(StackNode **top, ElemType *x) {//出栈操作
    StackNode *p;
    if(*top==NULL)
        printf("Stack is empty!\n");
    else{
        *x=(*top)->date;
        p=*top;
        *top=(*top)->next;
        free(p);
    }
    
}

int Calc(char *s) {
    StackNode *p;
    InitStack(&p);
    ElemType x1,x2;
    int i=0,x;
    while(s[i])//运算
    {
        if(s[i]>='0'&&s[i]<='9')
            Push(&p,s[i]-48);
        else{
            Pop(&p,&x1);
            Pop(&p,&x2);
            if(s[i]=='+')
                Push(&p,x1+x2);
            if(s[i]=='-')
                Push(&p,x1-x2);
            if(s[i]=='*')
                Push(&p,x1*x2);
            if(s[i]=='/')
                Push(&p,x1/x2);
        }
        i++;
    }
    return p->date;
}

int main() {
    char str[100] = "512+4*+3-";
    int rst;
    rst = Calc(str);
    printf("the results is: %d\n", rst);
    return 0;
}


```