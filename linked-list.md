# 链表的简单表达方式

>typedef struct node
{
    int elem;
    struct node *next;
}Node;

| 地址 | 0 | 100 | 200 | 300 | 400 | 500 | 600 |
---|---|---|---|---|---|---|---
| 内容（elem）| 鸡蛋 | 牛奶 | 苹果 |梨子| | | 啤酒|
| 下一个内容的地址（*next）|200|300|100|600| | | NULL|

鸡蛋->苹果->牛奶->梨子->啤酒->NULL；</br>
链表第一步首先需要创建一个盒子，盒子谁来创建，操作系统</br>
**malloc（？？**）</br>
## 放鸡蛋
malloc (sizeof(Node))-->int elem + *next;放置elem和 *next；</br>
Node *egg=malloc(sizeof(Node));</br>
egg->elem=鸡蛋;**把第一个元素鸡蛋放进egg盒子中；**</br>
egg->next=NULL;</br>
**block 是第一个盒子，保留这个盒子**
Node *head=egg;</br>
## 放苹果
Node *apple =malloc(sizeof(Node));</br>
apple ->elem = 苹果；</br>
apple->next=NULL; ***链表到目前位置结束链接***</br>
head->next = apple;  ***鸡蛋的下一个盒子指向的是装苹果的盒子***</br>
## 放牛奶

Node *milk = malloc(sizeof(Node));</br>
milk -> elem = 牛奶；</br>
milk ->next = NULL;</br>
apple ->next = milk;</br>

## 放梨子

Node *pear = malloc(sizeof(Node));</br>
pear -> elem = 梨子；</br>
pear -> next = NULL;</br>
milk -> next = pear;</br>

## 链表插入元素
- 插入链表的头部（头节点之后），作为首元节点；
- 插入链表中间的某个位置；
- 插入链表的最末端，作为链表的最后一个数据元素。

**不管是插入到链表的那个位置，操作一般分为两步**
1. 将新的节点的next指针指向插入位置后的节点
2. 将插入位置前的节点的next指针指向插入节点。

![add](https://img-camp.banyuan.club/cc002/chapter3/3-5.png?x-oss-process=image/resize,w_500/sharpen,100)

**如果先执行步骤二，即先将插入位置前节点的next指针先指向插入节点，会导致插入位置后续的部分链表丢失，无法实现步骤一**

## 链表删除元素
从链表中删除指定元素就是将存放该数据元素的节点从链表中删除，同时对于不再利用的存储空间还要及时的释放，因此在链表中删除数据元素需要进行以下两个步骤：

1. 将节点从链表中摘除；
2. 手动释放节点，回收被节点占用的存储空间。
![delet](https://img-camp.banyuan.club/cc002/chapter3/3-6.png?x-oss-process=image/resize,w_500/sharpen,100)