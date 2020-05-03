# C exercise
Write code to prepend (add to the start) to a linked list, and to free/empty the entire list.

```c
struct ll_node {
    struct ll_node* next; 
    int value; 
}
```
完成以下方法：
```c
void free_ll(struct ll_node** list) {

} 

void prepend(struct ll_node** list, int value) {

}
```

answer:

```c
void free_ll(struct ll_node** list) {
    if(*list) {
        free_ll(&((*list)->next));//go to the end
        free(*list);//free from the end
    }
    *list = NULL;
} 

void prepend(struct ll_node** list, int value) {
    struct ll_node* item = (struct ll_node*)malloc(sizeof(struct ll_node));
    item->value = value;
    item->next = *list;
    *list = item;
}
```