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

