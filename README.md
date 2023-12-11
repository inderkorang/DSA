# DSA
DSA end file
/* Write a C program to Insert and Delete elements form a Queue using link list ,each node 
 should have the following inforamaion about a product Product_Id(char) , Product_Name(string) , 
 Total_sale(integer),Product_Grade(Char)

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct product {
    char Product_Id;
    char Product_Name[100];
    int Total_sale;
    char Product_Grade;
    struct product *next;
};

struct product *front = NULL;
struct product *rear = NULL;

void enqueue(char id, char name[], int sale, char grade) {
    struct product *new_product = (struct product*) malloc(sizeof(struct product));
    new_product->Product_Id = id;
    strcpy(new_product->Product_Name, name);
    new_product->Total_sale = sale;
    new_product->Product_Grade = grade;
    new_product->next = NULL;

    if (rear == NULL) {
        front = rear = new_product;
        return;
    }
    rear->next = new_product;
    rear = new_product;
}

void dequeue() {
    if (front == NULL) {
        printf("Queue is empty\n");
        return;
    }
    struct product *temp = front;
    front = front->next;
    if (front == NULL) {
        rear = NULL;
    }
    free(temp);
}

void display() {
    if (front == NULL) {
        printf("Queue is empty\n");
        return;
    }
    struct product *temp = front;
    while (temp != NULL) {
        printf("Product_Id: %c, Product_Name: %s, Total_sale: %d, Product_Grade: %c\n", temp->Product_Id, temp->Product_Name, temp->Total_sale, temp->Product_Grade);
        temp = temp->next;
    }
}

int main() {
    enqueue('A', "Product A", 100, 'A');
    enqueue('B', "Product B", 200, 'B');
    enqueue('C', "Product C", 300, 'C');
    enqueue('D', "Product D", 400, 'D');
    display();
    printf("After a deletion\n ");
     dequeue();
    //display();
    dequeue();
    display();
    return 0;
}
*/

/* Q 2 . Let A and B be two structures of type Linked List. Write a ‘C ’ program to create a new
 Linked List ‘S’ that contains elements alternately from A and B beginning with the first
 element of A. If you run out of elements in one of the lists, then append the remaining

 #include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *next;
};

void insert(struct node **head, int data) {
    struct node *new_node = (struct node*) malloc(sizeof(struct node));
    new_node->data = data;
    new_node->next = *head;
    *head = new_node;
}

void merge(struct node **head1, struct node **head2, struct node **head3) {
    struct node *temp1 = *head1;
    struct node *temp2 = *head2;
    struct node *temp3 = NULL;

    while (temp1 != NULL && temp2 != NULL) {
        insert(&temp3, temp1->data);
        insert(&temp3, temp2->data);
        temp1 = temp1->next;
        temp2 = temp2->next;
    }

    while (temp1 != NULL) {
        insert(&temp3, temp1->data);
        temp1 = temp1->next;
    }

    while (temp2 != NULL) {
        insert(&temp3, temp2->data);
        temp2 = temp2->next;
    }

    *head3 = temp3;
}

void display(struct node *head) {
    while (head != NULL) {
        printf("%d ", head->data);
        head = head->next;
    }
}

int main() {
    struct node *A = NULL;
    struct node *B = NULL;
    struct node *S = NULL;

    insert(&A, 1);
    insert(&A, 3);
    insert(&A, 5);
    insert(&A, 7);
    insert(&A, 9);

    insert(&B, 2);
    insert(&B, 4);
    insert(&B, 6);
    insert(&B, 8);
    insert(&B, 10);

    merge(&A, &B, &S);

    printf("Linked List S: ");
    display(S);

    return 0;
}
*/

/*3.Write a C program to create a single linked list then input a value V, partition it such that all nodes less 
 than V come before nodes greater than or equal to V.
*/
#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *next;
};

void insert(struct node **head, int data) {
    struct node *new_node = (struct node*) malloc(sizeof(struct node));
    new_node->data = data;
    new_node->next = *head;
    *head = new_node;
}

void partition(struct node **head, int V) {
    struct node *smaller_head = NULL;
    struct node *smaller_tail = NULL;
    struct node *greater_head = NULL;
    struct node *greater_tail = NULL;
    struct node *current = *head;

    while (current != NULL) {
        if (current->data < V) {
            if (smaller_head == NULL) {
                smaller_head = current;
                smaller_tail = current;
            } else {
                smaller_tail->next = current;
                smaller_tail = current;
            }
        } else {
            if (greater_head == NULL) {
                greater_head = current;
                greater_tail = current;
            } else {
                greater_tail->next = current;
                greater_tail = current;
            }
        }
        current = current->next;
    }

    if (smaller_head == NULL) {
        *head = greater_head;
    } else {
        smaller_tail->next = greater_head;
        *head = smaller_head;
    }

    if (greater_tail != NULL) {
        greater_tail->next = NULL;
    }
}

void display(struct node *head) {
    while (head != NULL) {
        printf("%d ", head->data);
        head = head->next;
    }
}

int main() {
    struct node *head = NULL;

    insert(&head, 10);
    insert(&head, 4);
    insert(&head, 50);
    insert(&head, 30);
    insert(&head, 2);

    printf("Original Linked List: ");
    display(head);

    partition(&head, 10);

    printf("\nPartitioned Linked List: ");
    display(head);

    return 0;
}
