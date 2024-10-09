```
#include <stdio.h>
#include <stdlib.h>

struct ListNode {
    int data;                  
    struct ListNode* next;     
};


struct ListNode* init_list() {
    struct ListNode* head = (struct ListNode*)malloc(sizeof(struct ListNode));
    if (head == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    head->data = 1;           
    head->next = NULL;
    return head;
}

void print_list(struct ListNode* head) {
    struct ListNode* current = head;
    int count = 0;  
    while (current != NULL&&count < 200) {  
        printf("%d -> ", current->data);
        current = current->next;
        count++;
    }
    printf("NULL\n");
}

void T(struct ListNode* head, int data) {
    struct ListNode* new_node = (struct ListNode*)malloc(sizeof(struct ListNode));
    if (new_node == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    new_node->data = data;
    new_node->next = NULL;

    struct ListNode* current = head;
    while (current->next != NULL) {
        current = current->next;
    }
    current->next = new_node;  
}

void H(struct ListNode** head_ref, int data) {
    struct ListNode* new_node = (struct ListNode*)malloc(sizeof(struct ListNode));
    if (new_node == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    new_node->data = data;
    new_node->next = *head_ref;  
    *head_ref = new_node;        
}

void D(struct ListNode** head_ref, int location) {
    if (*head_ref == NULL || location <= 0) {
        printf("Invalid location!\n");
        return;
    }

    struct ListNode* current = *head_ref;

    if (location == 1) {
        *head_ref = current->next;  
        free(current);              
        return;
    }

    for (int i = 1; current != NULL && i < location - 1; i++) {
        current = current->next;
    }

    if (current == NULL || current->next == NULL) {
        printf("Location exceeds the length of the list.\n");
        return;
    }

    struct ListNode* temp = current->next;
    current->next = temp->next;  
    free(temp);  
}

void C(struct ListNode* head) {
    if (head == NULL) {
        printf("List is empty!\n");
        return;
    }

    struct ListNode* current = head;
    while (current->next != NULL) {
        current = current->next;
    }
    current->next = head;
}

int main() {
    struct ListNode* head = init_list();
    char command;
    int value1, value2, value3, location;

    printf("pleas input your command:\n");
    while (scanf(" %c", &command) != EOF) {
        switch (command) {
            case 'H':  // 在头部插入多个节点
                scanf("%d %d %d", &value1, &value2, &value3);
                H(&head, value3);
                H(&head, value2);
                H(&head, value1);
                break;
            case 'T':  // 在尾部插入多个节点
                scanf("%d %d %d", &value1, &value2, &value3);
                T(head, value1);
                T(head, value2);
                T(head, value3);
                break;
            case 'D':  // 删除指定位置的节点
                scanf("%d", &location);
                D(&head, location);
                break;
            case 'C':  // 将链表变为循环链表
                C(head);
                break;
            case 'P':  // 打印链表,因为是循环链表故显视20个节点避免无限循环
                print_list(head);
                break;
            default:
                printf("fault!\n");
                break;
        }
        printf("please input your command:\n");
    }

    return 0;
}
```



```
#include <stdio.h>
#include <stdlib.h>

typedef struct node{
 int data;
 struct node *next;
 }Node;

 Node *createList();
 void printNodeList(Node *node);
 void baoshu(Node *head);
 void circle(Node *head);
 int main()
 {
  Node *head = createList();
  printNodeList(head);
  circle(head);
  baoshu(head);
  return 0;
 }
 
 Node *createList(){
  Node *head = (Node *)malloc(sizeof(Node));
  if(head == NULL){
    return NULL;
  }
  head->next = NULL;
  head->data = 1;
 
  int num = -1;
  printf("please in put the data\n");
  scanf("%d", &num);
  while(num != -1){
    Node *cur = (Node *)malloc(sizeof(Node));
    cur->data = num;
    cur->next = head->next;
    head->next = cur;
    scanf("%d", &num);
  }
  return head;
 }
 
 void printNodeList(Node *node){
  Node *head = node->next;
  while(head != NULL){
    int currentData = head->data;
    printf("currentData = %i\n", currentData);
   head = head->next;
  }
 }

 void circle(Node *head){
    if (head == NULL) {
        printf("List is empty!\n");
        return;
    }
    Node *current = head;
     while (current->next != NULL) {
        current = current->next;
    }
    current->next = head;
 }

 void baoshu(Node *head) {
    if (head == NULL|| head->next == NULL){
        printf("list is empty or has only one node!\n");
        return;
    }
    int turn = 1;
    int count = 1;
    int i = 34;
    Node *current = head->next;
    while(current->data != 3){
        current = current->next;
    }
    Node *processedNodes[34];
    int processedCount = 0;

    Node *previous = head;
    while (previous->next != current){
        previous = previous->next;
    }
    while (i != 0){
        if(count == turn){
            Node *toDelete = current;
            int e = toDelete->data;
            processedNodes[processedCount++] = toDelete;
            previous->next = current->next;
            current = current->next;
            printf("%d ",e);
            count = 1;
            turn++;
            i--;
        }else{
            count++;
            previous = current;
            current = current->next;          
        }
    }
    for(int j = 0;j < processedCount;j++){
        free(processedNodes[j]);
    }
 }

```

 [Josephus.out](E:\新建文件夹\Josephus.out) 

```
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#define MaxSize 30

typedef struct {
    char data[MaxSize];
    int top;//栈顶指针
}stack;
//初始化栈顶指针
void initsatck(stack *z){
    z->top=-1;
}
//入栈 
bool input(stack *z, char x){
    if(z->top == MaxSize - 1){
        return false;
    }
    z->top=z->top + 1;
    z->data[z->top]=x;
    return true;
}
//出栈并输出 
bool out(stack *z){
    if(z->top == -1){
        return false;
    }
    printf("%c", z->data[z->top]);
    z->top = z->top - 1;
}
int main(){
    char original[] = "kiglnmrmeiahenrteof4ardar";
    int nums[] = {3,1,1,2,2,1,2,1,1,2,1,2,2,1,1,2,2,1,1,2,2,1,1,2,2,1,1,2,1,1,1,1,1,2,};

    stack z;
    initsatck( &z);
    int i = 0;
    int cnt = 0; 
    printf("%c\n", nums[i]);
    while(nums[i] != '\0'){
        // 奇数压入
        if((i+1) % 2 == 1){
            int j = 0;
            for(j=0; j<nums[i]; j++){
                input(&z,original[cnt++]);
            }
        }//偶数弹出 
        if((i+1) % 2 == 0){
            int j = 0;
            for(j=0; j<nums[i]; j++){
                out(&z); 
            }
        }
        i++;
    }
    return 0;
}
```

