# data-structure
其实就是我的数据结构学习笔记鸭
# 先给第二次作业写上

一、线性表可用于顺序表或链表存储,试问:

1. 两种存储表示各有哪些主要优缺点?

   1. 顺序存储表示是将数据元素存放于一个连续的存储空间中,实现顺序存取或(按下
      标)直接存取。它的存储效率高,存取速度快。但它的空间大小一经定义,在程序整个运行
      期间不会发生改变,因此,不易扩充。同时,由于在插入或删除时,为保持原有次序,平均
      需要移动一半(或近一半)元素,修改效率不高
      链接存储表示的存储空间一般在程序的运行过程中动态分配和释放,且只要存储器中还
      有空间,就不会产生存储溢出的问题。同时在插入和删除时不需要保持数据元素原来的物理
      顺序,只需要保持原来的逻辑顺序,因此不必移动数据,只需修改它们的链接指针,修改效
      率较高。但存取表中的数据元素时,只能循链顺序访问,因此存取效率不高。

2. 如果有n个表同时并存,并且在处理过程中各表的长度会动态发生变化,表的
   总数也可能自动改变,在此情况下,应选用哪种存储表示?为什么?

   1. 如果有n个表同时并存,并且在处理过程中各表的长度会动态发生变化,表的总数
      也可能自动改变、在此情况下,应选用链接存储表示。
      如果采用顺序存储表示,必须在一个连续的可用空间中为这n个表分配空间。初始时因
      不知道哪个表增长得快,必须平均分配空间。在程序运行过程中,有的表占用的空间增长得
      快,有的表占用的空间增长得慢;有的表很快就用完了分配给它的空间,有的表才用了少量
      的空间,在进行元素的插入时就必须成片地移动其他的表的空间,以空出位置进行插入;在
      元素删除时,为填补空白,也可能移动许多元素。这个处理过程极其繁琐和低效。
      如果采用链接存储表示,一个表的存储空间可以连续,可以不连续。表的增长通过动态
      存储分配解决,只要存储器未满,就不会有表溢出的问题;表的收缩可以通过动态存储释放
      实现,释放的空间还可以在以后动态分配给其他的存储申请要求,非常灵活方便。对于n
      个表(包括表的总数可能变化)共存的情形,处理十分简便和快捷。所以选用链接存储表示较
      好

3. 若表的总数基本稳定,且很少进行插入和删除,但要求以最快的速度存取表中
   的元素,应采用哪种存储表示?为什么?

   1. 应采用顺序存储表示。因为顺序存储表示的存取速度快,但修改效率低。若表的总
      数基本稳定,且很少进行插入和删除,但要求以最快的速度存取表中的元素,这时采用顺序
      存储表示较好。

      ------

      

      二、利用顺序表的操作,实现以下函数:

      1. 从顺序表中删除具有最小值的元素并由函数返回被删元素的值,空出的位置由
         最后一个元素填补。
      2. 从顺序表中删除具有给定值x的所有元素。
      3. 从有序顺序表中删除其值在给定值s与t之间(s<t)的所有元素。

```c++
#include <iostream>
using namespace std;
//第二题
template <typename T>
T delMin(T *Target, int &maxSize) {
    T Min = Target[0];
    int Pos = 0;
    for(int i = 1; i < maxSize; i++)
        if(Min > Target[i])
            Min = Target[i], Pos = i;
    Target[Pos] = Target[maxSize - 1];
    maxSize--;
    return Min;
}
template <typename T>
void delTag(T *Target, T x, int &maxSize) {
    for(int i = 0, j; i < maxSize; i++)
        if(Target[i] == x)
            for(maxSize--, j = i, i--; j < maxSize - 1; j++)
                Target[j] = Target[j + 1];
}
template <typename T>
void delRange(T *Target, T s, T t, int &maxSize) {
    for(int i = 0, j; i < maxSize; i++)
        if(Target[i] <= t && Target[i] >= s)
            for(maxSize--, j = i, i--; j < maxSize - 1; j++)
                Target[j] = Target[j + 1];
}


```

三、给定一个不带头结点的单链表,写出将链表倒置的算法。

```c++
#include<iostream>
using namespace std;
typedef int DataType;

struct Node{
    DataType data;
    Node *next;
};

class LinkList{
private:
    Node *head,*tail;
public:
    LinkList()
    {
        head = NULL;//不带头结点
    }
    void Inset(DataType a);
    void show();
    void reverse();
    ~LinkList(){//循环析构头结点
        Node *p=0;
        while(head)
        {
            p = head;
            head = head->next;
            delete p;   
        };
    }
};

void LinkList::show()
{
    Node *p = head;
    while(p)
    {
        cout<<p->data<<' ';
        p = p->next;
    }
    cout<< endl;
}

void LinkList::reverse()//1->2->3->4,1<-2 3->4,1<-2<-3 4,1<-2<-3<-4
{
    Node *p,*q;
    if(head && head->next)
    {
        p=head;
        q=p->next;
        p->next=NULL;//断开头指针
        while(q)
        {
            p=q;
            q=q->next;//p,q往后挪一位
            p->next=head;//next指向上一个节点
            head=p;//头结点往后挪一位
        }
    }
}

void LinkList::Inset(DataType a)
{   
    Node *s = new Node;
    s->data = a;
    if(!head)//判断插入的是否为第一个节点
    {
        head = tail = s;
    }
    else
    {
        tail->next=s;
        tail = s;
        s->next=NULL;
    }
}

int main()
{
    LinkList list;
    int n = 0, i = 0, j;
    cout<<"请输入单链表中的节点个数：";
    cin>>n;
    cout<<"请输入所要建立的单链表所包含的元素："<< endl;
    while(i<n)
    {
        cin>>j;
        list.Inset(j);
        i++;
    }
    list.show();
    list.reverse();
    list.show();
    return 0;
}
```

4.已知head为单链表的表头指针,链表中存储的都是整形数据,实现下列运算的
递归算法:

1. 求链表中的最大值。
2. 求链表中的结点个数。
3. 求所有整数的平均值。

```c++
#include<iostream>
using namespace std;
struct Node
{
	int num;
	Node* next;
};


Node * Creat(int a[],int n)
{
	int i;
	Node *head=(Node*)malloc(sizeof(Node));
	head->num=a[0];
	Node *pre=head;
	Node *p;
	for(i=1;i<n;i++)
	{
		p=(Node*)malloc(sizeof(Node));
		p->num=a[i];
		p->next=pre->next;
		pre->next=p;
		pre=p;
	}
    p->next=NULL;
	return head;
}

int Find(Node *head,int n)
{
	int flat=head->num;
	int i=0;
	for(;i<n;head=head->next,i++)
	{
		if(flat<head->num)
			flat=head->num;
	}
	return flat;
}

Node * Delete(Node * head,Node * pDel,Node* pre)
{
	if(pDel==head)
		head=pDel->next;
	pre->next = pDel->next;
	pDel=NULL;
	free(pDel);
	return head;
}


int main()
{
	int n,s=0,a=0;
	cin>>n;    //你要输入链表的长度
	int *p=new int[n];
	for(int i=0; i<n;i++)
	{
	cin>>p[i];
	s=s+p[i];
	a++;
	}   // 输入你要的链表
	Node *head=Creat(p,n);  //创建链表

	int max=Find(head,n);   //找到最大值
	cout<<"链表中的最大值是"<<max<<endl;
	cout<<"所有整数的和是"<<s<<endl;
	cout<<"所有整数的平均值是"<<s/n<<endl;
	cout<<"结点数是"<<a<<endl;
	delete [] p;
	return 0;

} 


```

5.设A和B是两个单链表,其表中元素递增有序。试写一算法将A和B归并成
个按元素值递减有序的单链表C,并要求辅助空间为O(1),试分析算法的时间复杂度。

```c++
template <typename T>
List<T>* Merge(List<T> *A, List<T> *B, List<T>* &C) {
    if(A == NULL && B == NULL)
        return C;
    if(A == NULL || (B != NULL && A -> Value > B -> Value))
        return Merge(A, B -> next, C) -> next = B;
    if(B == NULL || (A != NULL && A -> Value < B -> Value))
        return Merge(A -> next, B, C) -> next = A;
}
//时间复杂度为O（LEN（A） + LEN（B））

```

6.设ha和hb分别是两个带头结点的非递减有序单链表的表头指针,试设计一个
算法,将这两个有序链表合并成一个非递减有序的单链表。要求结果链表仍使用原来两
个链表的存储空间,不另外占用其他的存储空间。表中允许有重复的数据。

```c++
//第六题
template <typename T>
List<T>* Merge(List<T> *hA, List<T> *hB) {
    if(!hA -> Value && !hB -> Value) {
        hA = hA -> next, hB = hB -> next;
        return Merge(hA, hB);
    }
    if(hA == NULL && hB == NULL)
        return NULL;
    if(hA == NULL || (hB != NULL && hA -> Value > hB -> Value)) {
        hB -> next = Merge(hA, hB -> next);
        return hB;
    }
    if(hB == NULL || (hA != NULL && hA -> Value < hB -> Value)) {
        hA -> next = Merge(hA -> next, hB);
        return hA;
    }
}


```

7.设双链表表示的线性表L=(a1,a2,,.n),试写一时间复杂度为O(n)的算法,将L
改造为L=a1,ay,,an,,a4,a2)。

```c++
//第七题
//7
template <class T>
class doubleList {
public :
    doubleList *Next, *Back;
    T Value;
    List() {
        Next = NULL, Back = NULL;
        Value = 0;
    }
    ~doubleList() {
        delete Next, Back;
    }
};
template <typename T>
doubleList<T>* Merge(doubleList<T> *L) {
    if(L -> Next == 0)
        return L;
    doubleList<T> *left = L;
    doubleList<T> *right = L -> Next;
    doubleList<T> *Temp = right;
    while(Temp -> Next) {
        doubleList<T> *Next_1 = Temp -> next;
        if(!Next_1 -> Next) {
            left -> Next = Next_1, Next_1 -> Back = left, Next_1 -> Next = right, right -> Back = Next_1, Temp -> Next = NULL;
            break;
        }
        doubleList<T> *Next_2 = Next_1 -> Next;
        Temp -> Next = Next_2 -> Next;
        left -> Next = Next_1, Next_1 -> Back = left, left = left -> Next;
        Next_2 -> Next = right, right -> Back = Next_2, right = right -> Back;
    }
    return L;
}


```
