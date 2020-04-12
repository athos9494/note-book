# 队列（queue）
**顺序队列的不足之处以及解决办法**
假设一个队列有n个元素，顺序队列需建立一个大于n的数组，并把队列的所有元素存储在数组的前n个单元，数组下标为0的一端即是队头。
为了避免当只有一个元素时，队头和队尾重合使处理变得麻烦，所以引入两个指针，top指针指向队头元素，end指针指向队尾元素的下一个位置，这样当top等于end时，而是空队列。
|top/end| | | | |
|---|---|---|---|---|
| nothing| | | | |
|0|1|2|3|4|

|top| | | | end|
|---|---|---|---|---|
| data1|data2 |data3 |data4 | |
|0|1|2|3|4|

比如长度5的数组a，空队列时top和end指针指向的都是a[0],如果数组放入4个元素后top指向的还是a[0],但是end指向的变成了a[4].
假如数据完全出队以后，top指向的就是a[4],end指向的就是a[4]

| | | | | top/end|
|---|---|---|---|---|
| nothing| | || |
|0|1|2|3|4|

此时如果继续入队操作，end指针指向的位置就会超过数组长度限制，产生数组越界的错误，但是之前a[0]到a[3]的位置还是空的

顺序队列的缺点：空队时top和end都指向下标a[0]处，进队时end先+1，再放入数据，出队时top先+1，再删数据
队满时一个个删数据，当top和end都指向最后的位置时，前面的空间空余出来不能再使用。，为了解决这种问题引入的循环队列
在循环队列中，当队列为空时，有top=end，而当所有队列空间全占满时，也有top=end。为了区别这两种情况，规定循环队列最多只能有MaxSize-1个队列元素，当循环队列中只剩下一个空存储单元时，队列就已经满了。因此，队列判空的条件是top=end，而队列判满的条件是top=（end+1)%MaxSize。

**leetcode**
```
#include <stdio.h>
#include <stdlib.h>
typedef struct node
{
    int data;
    struct node *pNext;
}Node;
typedef struct
{
    Node *tail;
    Node *head;//链表表示队列
    int len;//队列的长度
    
} RecentCounter;
void enQueue(RecentCounter *queue,int elem);
int deQqueue(RecentCounter *queue);
int peek(RecentCounter *queue);

RecentCounter *recentCounterCreate(void);
int recentCounterPing(RecentCounter *obj,int t);
void recentCounterFree(RecentCounter* obj);

RecentCounter* recentCounterCreate(void)
{
    RecentCounter *p = (RecentCounter *)malloc(sizeof(RecentCounter));
    p->head = p->tail = NULL;
    p->len = 0;
    return p;
    
}

void enQueue(RecentCounter *queue,int elem)
{
    Node *pnewNode = (Node *)malloc(sizeof(Node));
    pnewNode->data = elem;
    pnewNode->pNext = NULL;
    if (queue->tail == NULL)
    {
        queue->tail = pnewNode;
        queue->head = pnewNode;
    }
    else
    {
        queue->tail->pNext = pnewNode;
        queue->tail = pnewNode;
    }
    queue->len++;
}

int deQqueue(RecentCounter *queue)
{
    Node *temp = queue->head;
    queue->head = temp->pNext;
    if(queue->head == NULL)
    {
        queue->tail = NULL;
    }
    queue->len--;
    int r = temp->data;
    free(temp);
    return r;
}


int peek(RecentCounter *queue)
{
    return queue->head->data;
}



int recentCounterPing(RecentCounter* obj, int t)
{
    enQueue(obj, t);
    while(peek(obj)<(t-3000))
    {
        deQqueue(obj);
    }
    return obj->len;
    
}
void recentCounterFree(RecentCounter* obj) {
    
    while(obj->len > 0)
    {
        deQueue(obj);
        
    }
    free(obj);
}
```

## 预习

- 树：存储一对多集合的数据元素
- 结点：使用树结构存储的每一个数据元素都叫结点，上下级叫父子结点，同一个父结点下的子结点互为兄弟结点，最上级的父结点就是根结点。
- 度：一个结点有的子树数称为结点的度，结点拥有的分枝数即为度，各个结点的最大分枝数就是整个树的度
- 层次：根结点第一层，以此类推，每个兄弟结点群位一层
- 有序树：树结点的子树从左往右看，左右规定划分，即为有序树。
- 二叉树：有序树，树结点每个分枝结点不超过2个
- 二叉树第i层最多2<sup>i-1</sup>个结点
- 度为k的二叉树，结点最多2<sup>k</sup>-1
- 二叉树终端结点树为n<sub>0</sub>,度为2 的结点数为n<sub>2</sub>,n<sub>0</sub>=n<sub>2</sub>+1;
- 满二叉树，每个结点的度都是2，此二叉树就是满二叉树
- 存储结构：顺序存储，链式存储。
问题：满二叉树与完全二叉树的增删操作区别是否相同；
怎么顺着结点一层一层打印结点数据；
怎么精确的找到结点的数据