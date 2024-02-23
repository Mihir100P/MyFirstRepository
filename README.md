# infix to postfix conversion
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<ctype.h>

#define n 100
struct stack{
    int top;
    char *Sarr;
};
struct Queue{
    int front;
    int rear;
    char *Qarr;
};
struct stack *InitializeStack(int capacity){
    struct stack *s =(struct stack*)malloc(sizeof(struct stack));
    s->Sarr=(char*)malloc(sizeof(char)*capacity);
    s->top=-1;
    return s;
}
struct Queue *InitializeQueue(int capacity){
    struct Queue *q =(struct Queue*)malloc(sizeof(struct Queue));
    q->front=q->rear=-1;
    q->Qarr=(char*)malloc(sizeof(char)*capacity);
    return q;
}
char *InfixExp(){
    char *Infix = (char *)malloc(sizeof(char) * n);
    printf("Enter Infix Expression : ");
    scanf("%s",Infix);
    return Infix;
}
void Enqueue(char x,struct Queue *q,int capacity){
    if(q->rear==capacity-1){
        printf("Queue is FULL!\n");
    }
    else{
        if(q->front==-1&&q->rear==-1){
        q->front++;
        q->rear++;
        q->Qarr[q->rear]=x;
    }
    else{
        q->rear=q->rear+1;
        q->Qarr[q->rear]=x;
    }
    }
}
void pop(char x,struct stack *s,struct Queue *q,int capacity){
    if(s->top==-1){
        printf("Empty!");
    }
    else{
    Enqueue(x,q,capacity);
    s->top--;
    return;
    }
}
int push(char x,struct stack *s,int capacity){
    if(s->top==capacity-1){
        printf("Full!\n"); 
    }
    else if(x==')' || x=='('){
    s->top++;
    s->Sarr[s->top]=x;
    return s->top;
    }
    else{
    s->top++;
    s->Sarr[s->top]=x;
    }
}
int prec(char x){
    if(x=='(' || x==')'){
        return 0;
    }
    if(x=='^'){
        return 1;
    }
    if(x=='*' || x=='/'){
        return 2;
    }
    if(x=='+' || x=='-'){
        return 3;
    }
    return -1;
}
void PostFixExp(char *Infix,struct Queue *q,int capacity,struct stack *s){
    for(int i=0;i<strlen(Infix);i++){
        if(isalnum(Infix[i])){
            Enqueue(Infix[i],q,capacity);
        }
        else if(Infix[i]=='('){
            push(Infix[i],s,capacity);
        }
        else if(Infix[i]==')'){
            while(s->top!=-1 && s->Sarr[s->top]!='('){
                pop(s->Sarr[s->top],s,q,capacity);
            }
             if(s->top != -1 && s->Sarr[s->top] == '(') {
               s->top--;
           }
        }
        else{
            int newop=prec(Infix[i]);
            if(s->top!=-1 && newop<=prec(s->Sarr[s->top])){
                pop(s->Sarr[s->top],s,q,capacity);
            }
            else{
                push(Infix[i],s,capacity);
            }
            }
        }
       while (s->top != -1){
       pop(s->Sarr[s->top], s, q, capacity);
       }
    printf("PostifixExpression is : ");
    for(int j=q->front;j<=q->rear;j++){
        printf("%c",q->Qarr[j]);
    }
}
int main()
{
    int capacity=100;
    struct stack *s=InitializeStack(capacity);
    struct Queue *q=InitializeQueue(capacity);
    char *Infix=InfixExp();
    PostFixExp(Infix,q,capacity,s);
    free(s->Sarr);
    free(q->Qarr);
    free(s);
    free(q);

return 0;
}




