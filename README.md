# -#include<stdlib.h>
#include<string>
#include<iostream>
#include<ctime>
#include<windows.h>
using namespace std;
static int I = 0;//猪圈序号
static int i = 0;//猪的序号
static string a = "黑猪";
static string b = "小花猪";
static string c = "大花白猪";
static string d = "空";
typedef struct Pig{//猪
    int pigstynum;
    int num;
    char *type;
    double weight;
    double time;
}pig;

typedef struct Pigsty{//猪圈
    int no;
    int pignum;
    bool black;
    struct node *a;
}pigsty;

typedef struct Node//猪圈指针
{
    pigsty Data;
    struct Node *next;
}Lnode;

typedef struct node//猪指针
{
    pig data;
    struct node *next;
}lnode;

void Clean(Lnode *p){//将猪圈清空
    Lnode *r = p;
    r->Data.black = 0;
    r->Data.no = 0;
    r->Data.pignum = 0;
}

void clean(lnode *p){//将猪槽清空
    lnode *r = p;
    r->data.num = 0;
    r->data.pigstynum = 0;
    r->data.time = 0;
    r->data.type = &d[0];
    r->data.weight = 0;
}

void creat(lnode *h,int n)//生成猪槽
{
    lnode *p,*r;
    int j;
    r=h;
    for(j=0;j<n;j++)
    {
        p=(lnode *)malloc(sizeof(lnode));
        r->next=p;
        clean(r);
        r->data.pigstynum = I;
        r->data.num = i;
        i++;
        if(i==10) i = i - 10;
        r=p;
    }
    I++;
    r->next=NULL;
}

void Creat(Lnode *h,int n)//生成猪圈
{
    Lnode *p,*r;
    int j;
    r=h;
    for(j=0;j<n;j++)
    {
        p=(Lnode *)malloc(sizeof(Lnode));
        r->next=p;
        Clean(r);
        r->Data.no = I;
        lnode *hh;
        hh=(lnode *)malloc(sizeof(lnode));
        hh->next=NULL;
        r->Data.a = hh;
        creat(hh, 10);
        r=p;
    }
    r->next=NULL;
}

void random(pig u)//随机生成一个猪
{
    double weight;
    weight=rand() % 30;
    weight=weight+20.00;
    u.weight = weight;
    int tpnum;
    tpnum = rand() % 3;
    switch(tpnum){
        case 0:
            u.type = &a[0];
            break;
        case 1:
            u.type = &b[0];
            break;
        case 2:
            u.type = &c[0];
            break;
        };
}

void Input(Lnode *p,pig u)//放1猪进猪圈
{
    random(u);
    for (int j = 0; j < 10;j++){
        Lnode *r = p;
        for (; r->next != NULL; r = r->next)
        {
            if (r->Data.pignum <= j){
                if (r->Data.pignum==0){
                    if (u.type==&a[0]) r->Data.black = 1;
                }
                while(r->Data.pignum!=0&&r->Data.black==0){
                    if(u.type==&a[0]) r = r->next;
                }
                while(r->Data.black==1){
                    if (u.type!=&a[0]) r = r->next;
                }
            lnode *h = p->Data.a;
            for (; h->next != NULL; r = r->next){
                while (h->data.weight != 0)
                    h = h->next;
                while(h->data.weight==0){
                h->data.weight = u.weight;
                h->data.type = u.type;
                h->data.pigstynum = p->Data.no;
                r->Data.pignum++;
                }
            }    
            }
        }
        delete r;
    }
}

void output(Lnode *h)//输出
{
    Lnode *p;
    p = h;
    while(p->next!=NULL)
    {
        cout<<p->Data.no<<endl;
        lnode *r = p->Data.a;
        while(r->next!=NULL){
            cout << r->data.num << r->data.type << r->data.weight << " ";
            r=r->next;
        }
        cout << endl;
        p=p->next;
    }
    delete p;
}

int main()
{
    Lnode *b;
    b=(Lnode *)malloc(sizeof(Lnode));
    b->next = NULL;
    Creat(b,100);
    int n = rand() % 100;
    while(n==0){
        n = rand() % 100;
    }
    pig u;
    //for (int j = 0; j < n;j++) 
    Input(b,u);
    output(b);
    return 0;
}
