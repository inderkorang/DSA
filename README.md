# DSA
DSA end file
/* Write a C program to Insert and Delete elements form a Queue using link list ,each node 
 should have the following inforamaion about a product Product_Id(char) , Product_Name(string) , 
 Total_sale(integer),Product_Grade(Char)*/

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


/* Q 2 . Let A and B be two structures of type Linked List. Write a ‘C ’ program to create a new
 Linked List ‘S’ that contains elements alternately from A and B beginning with the first
 element of A. If you run out of elements in one of the lists, then append the remaining
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

/*Q4  Write a C program to create two single linked lists, and then write another function to subtract two numbers represented as linked list.
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};

void push(struct Node** head_ref, int new_data) {
    struct Node* new_node = (struct Node*)malloc(sizeof(struct Node));
    new_node->data = new_data;
    new_node->next = (*head_ref);
    (*head_ref) = new_node;
}

void printList(struct Node* node) {
    while (node != NULL) {
        printf("%d ", node->data);
        node = node->next;
    }
    printf("\\n");
}

struct Node* subtractLinkedList(struct Node* l1, struct Node* l2) {
    struct Node* res = NULL;
    struct Node *temp, *prev = NULL;
    int borrow = 0, diff;

    while (l1 != NULL || l2 != NULL) {
        diff = (l1 ? l1->data : 0) - (l2 ? l2->data : 0) - borrow;
        if (diff < 0) {
            diff += 10;
            borrow = 1;
        } else {
            borrow = 0;
        }
        temp = (struct Node*)malloc(sizeof(struct Node));
        temp->data = diff;
        temp->next = NULL;
        if (res == NULL) {
            res = temp;
        } else {
            prev->next = temp;
        }
        prev = temp;
        if (l1) {
            l1 = l1->next;
        }
        if (l2) {
            l2 = l2->next;
        }
    }
    if (borrow > 0) {
        temp = (struct Node*)malloc(sizeof(struct Node));
        temp->data = borrow;
        temp->next = NULL;
        prev->next = temp;
    }
    return res;
}

int main() {
    struct Node* head1 = NULL;
    struct Node* head2 = NULL;

    push(&head1, 9);
    push(&head1, 9);
    push(&head1, 9);
    printf("First List: ");
    printList(head1);

    push(&head2, 1);
    push(&head2,2);
    printf("Second List: ");
    printList(head2);

    struct Node* res = subtractLinkedList(head1, head2);
    printf("Resultant List: ");
    printList(res);

    return 0;
}*/

/* 6Write a C program to craeate a single linked list , like L0 -> L1 -> … -> Ln-1 -> Ln. Write another C
fucntion to rearrange the nodes in the list so that the new formed list is : L0 -> Ln -> L1 -> Ln-1 -> L2
-> Ln-2 .You are required to do this in place without altering the nodes’ values.

#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node* next;
};

void createList(struct node** head, int n) {
    struct node* temp = NULL;
    struct node* p = *head;
    for (int i = 0; i < n; i++) {
        temp = (struct node*)malloc(sizeof(struct node));
        printf("Enter the data for node %d: ", i + 1);
        scanf("%d", &temp->data);
        temp->next = NULL;
        if (*head == NULL) {
            *head = temp;
            p = *head;
        } else {
            p->next = temp;
            p = p->next;
        }
    }
}

void rearrangeList(struct node** head, int n) {
    struct node* p = *head;
    struct node* q = *head;
    struct node* prev = NULL;
    for (int i = 0; i < n / 2; i++) {
        prev = q;
        q = q->next;
    }
    prev->next = NULL;
    struct node* r = NULL;
    while (q != NULL) {
        r = q->next;
        q->next = p->next;
        p->next = q;
        p = q->next;
        q = r;
    }
}

void displayList(struct node* head) {
    while (head != NULL) {
        printf("%d -> ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}

int main() {
    int n;
    struct node* head = NULL;
    printf("Enter the number of nodes: ");
    scanf("%d", &n);
    createList(&head, n);
    printf("Original list: ");
    displayList(head);
    rearrangeList(&head, n);
    printf("Rearranged list: ");
    displayList(head);
    return 0;
}*/

/* 6 Write a C program to create two link lists positive and negative from Original linked list, so that
positive linked list contains all positive elements and negative linked list contains negative elements.
Positive and negative linked lists should use the node of existing original linked list.

#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node* next;
};

void splitList(struct node* head, struct node** posHead, struct node** negHead) {
    struct node* posTail = *posHead = NULL;
    struct node* negTail = *negHead = NULL;
    struct node* curr = head;

    while (curr != NULL) {
        if (curr->data >= 0) {
            if (*posHead == NULL) {
                *posHead = posTail = curr;
            } else {
                posTail = posTail->next = curr;
            }
        } else {
            if (*negHead == NULL) {
                *negHead = negTail = curr;
            } else {
                negTail = negTail->next = curr;
            }
        }
        curr = curr->next;
    }

    if (posTail != NULL) {
        posTail->next = NULL;
    }
    if (negTail != NULL) {
        negTail->next = NULL;
    }
}

int main() {
    struct node* head = NULL;
    struct node* posHead = NULL;
    struct node* negHead = NULL;

    // Create a sample linked list
    head = (struct node*)malloc(sizeof(struct node));
    head->data = -1;
    head->next = (struct node*)malloc(sizeof(struct node));
    head->next->data = 2;
    head->next->next = (struct node*)malloc(sizeof(struct node));
    head->next->next->data = -3;
    head->next->next->next = (struct node*)malloc(sizeof(struct node));
    head->next->next->next->data = 4;
    head->next->next->next->next = NULL;

    // Split the linked list into two separate lists
    splitList(head, &posHead, &negHead);

    // Print the positive linked list
    printf("Positive Linked List: ");
    while (posHead != NULL) {
        printf("%d ", posHead->data);
        posHead = posHead->next;
    }
    printf("\n");

    // Print the negative linked list
    printf("Negative Linked List: ");
    while (negHead != NULL) {
        printf("%d ", negHead->data);
        negHead = negHead->next;
    }
    printf("\n");

    return 0;
}*/

/*W.A.P. to create a binary search tree and perform following operations:
1) Search a particular key.
2) Delete a node from the tree.
3) Find total number of leaf nodes
4) Find height of a binary search tree
5) Count total numbers of nodes from right hand side of root node
6) Kth largest element without doing any modification in Binary Search Tree 
 
 #include <stdio.h>
#include <stdlib.h>

struct Node {
    int key;
    struct Node* left;
    struct Node* right;
};

struct Node* createNode(int key) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->key = key;
    newNode->left = newNode->right = NULL;
    return newNode;
}

struct Node* insert(struct Node* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }
    if (key < root->key) {
        root->left = insert(root->left, key);
    } else if (key > root->key) {
        root->right = insert(root->right, key);
    }
    return root;
}

struct Node* search(struct Node* root, int key) {
    if (root == NULL || root->key == key) {
        return root;
    }
    if (key < root->key) {
        return search(root->left, key);
    }
    return search(root->right, key);
}

struct Node* minValueNode(struct Node* node) {
    struct Node* current = node;
    while (current->left != NULL) {
        current = current->left;
    }
    return current;
}

struct Node* deleteNode(struct Node* root, int key) {
    if (root == NULL) {
        return root;
    }
    if (key < root->key) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->key) {
        root->right = deleteNode(root->right, key);
    } else {
        if (root->left == NULL) {
            struct Node* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct Node* temp = root->left;
            free(root);
            return temp;
        }
        struct Node* temp = minValueNode(root->right);
        root->key = temp->key;
        root->right = deleteNode(root->right, temp->key);
    }
    return root;
}

int countLeafNodes(struct Node* root) {
    if (root == NULL) {
        return 0;
    }
    if (root->left == NULL && root->right == NULL) {
        return 1;
    }
    return countLeafNodes(root->left) + countLeafNodes(root->right);
}

int height(struct Node* root) {
    if (root == NULL) {
        return 0;
    }
    int leftHeight = height(root->left);
    int rightHeight = height(root->right);
    return (leftHeight > rightHeight) ? (leftHeight + 1) : (rightHeight + 1);
}

int countNodesRightOfRoot(struct Node* root) {
    if (root == NULL) {
        return 0;
    }
    return 1 + countNodesRightOfRoot(root->left) + countNodesRightOfRoot(root->right);
}

void kthLargestUtil(struct Node* root, int k, int* count, int* result) {
    if (root == NULL || *count >= k) {
        return;
    }

    kthLargestUtil(root->right, k, count, result);
    if (*count < k) {
        *count = *count + 1;
        if (*count == k) {
            *result = root->key;
            return;
        }
        kthLargestUtil(root->left, k, count, result);
    }
}

int kthLargest(struct Node* root, int k) {
    int count = 0;
    int result = -1;
    kthLargestUtil(root, k, &count, &result);
    return result;
}

// Example usage:
int main() {
    struct Node* root = NULL;
    int keys[] = {50, 30, 70, 20, 40, 60, 80};
    int numKeys = sizeof(keys) / sizeof(keys[0]);

    for (int i = 0; i < numKeys; i++) {
        root = insert(root, keys[i]);
    }

    // 1) Search
    int searchKey = 40;
    struct Node* searchResult = search(root, searchKey);
    if (searchResult != NULL) {
        printf("Key %d found in the BST.\n", searchKey);
    } else {
        printf("Key %d not found in the BST.\n", searchKey);
    }

    // 2) Delete
    int deleteKey = 30;
    root = deleteNode(root, deleteKey);
    printf("Node with key %d deleted from the BST.\n", deleteKey);

    // 3) Count total number of leaf nodes
    int leafNodesCount = countLeafNodes(root);
    printf("Total number of leaf nodes: %d\n", leafNodesCount);

    // 4) Find height of the binary search tree
    int treeHeight = height(root);
    printf("Height of the BST: %d\n", treeHeight);

    // 5) Count total numbers of nodes from the right hand side of the root node
    int nodesCountRightOfRoot = countNodesRightOfRoot(root);
    printf("Total number of nodes to the right of the root: %d\n", nodesCountRightOfRoot);

    // 6) Kth largest element
    int k = 2;
    int kthLargestElement = kthLargest(root, k);
    printf("%dth largest element in the BST: %d\n", k, kthLargestElement);

    return 0;
}*/

/* 9 Write a C program to sort a sequence of characters given by user in an array, using Quick sort
technique.
#include <stdio.h>
#include <string.h>

void swap(char* a, char* b) {
    char t = *a;
    *a = *b;
    *b = t;
}

int partition(char arr[], int low, int high) {
    char pivot = arr[high];
    int i = (low - 1);

    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}

void quickSort(char arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    char arr[100];
    printf("Enter a sequence of characters: ");
    scanf("%s", arr);
    int n = strlen(arr);
    quickSort(arr, 0, n - 1);
    printf("Sorted sequence of characters: %s\n", arr);
    return 0;
}*/


/* 10. 
#include <stdio.h>
#include <stdlib.h>

struct node {
    int process_id;
    int time_left;
    struct node* next;
};

void addProcess(struct node** head, int process_id) {
    struct node* new_node = (struct node*)malloc(sizeof(struct node));
    new_node->process_id = process_id;
    new_node->time_left = 10;
    new_node->next = NULL;

    if (*head == NULL) {
        *head = new_node;
        new_node->next = *head;
    } else {
        struct node* last = *head;
        while (last->next != *head) {
            last = last->next;
        }
        last->next = new_node;
        new_node->next = *head;
    }
}

void deleteProcess(struct node** head, struct node* prev, struct node* curr) {
    if (prev == NULL) {
        *head = curr->next;
    } else {
        prev->next = curr->next;
    }
    free(curr);
}

void printCompletionTime(struct node* head) {
    int time_elapsed = 0;
    struct node* curr = head;
    struct node* prev = NULL;

    while (curr != NULL) {
        if (curr->time_left <= 0) {
            printf("Process %d completed in %d ms\n", curr->process_id, time_elapsed);
            deleteProcess(&head, prev, curr);
            curr = prev->next;
        } else {
            curr->time_left -= 10;
            prev = curr;
            curr = curr->next;
        }
        time_elapsed += 10;
    }
}

int main() {
    struct node* head = NULL;

    // Add processes to the circular linked list
    addProcess(&head, 1);
    addProcess(&head, 2);
    addProcess(&head, 3);
    addProcess(&head, 4);

    // Print the completion time of each process
    printCompletionTime(head);

    return 0;
}
*/

/*11 Write a C program to store the details of a directed weighted graph (Use array of pointers concept). 
#include <stdio.h>
#include <stdlib.h>

struct node {
    int vertex;
    int weight;
    struct node* next;
};

struct Graph {
    int numVertices;
    struct node** adjLists;
};

struct node* createNode(int v, int w) {
    struct node* newNode = malloc(sizeof(struct node));
    newNode->vertex = v;
    newNode->weight = w;
    newNode->next = NULL;
    return newNode;
}

struct Graph* createGraph(int vertices) {
    struct Graph* graph = malloc(sizeof(struct Graph));
    graph->numVertices = vertices;

    graph->adjLists = malloc(vertices * sizeof(struct node*));

    int i;
    for (i = 0; i < vertices; i++) {
        graph->adjLists[i] = NULL;
    }

    return graph;
}

void addEdge(struct Graph* graph, int src, int dest, int weight) {
    struct node* newNode = createNode(dest, weight);
    newNode->next = graph->adjLists[src];
    graph->adjLists[src] = newNode;
}

void printGraph(struct Graph* graph) {
    int v;
    for (v = 0; v < graph->numVertices; v++) {
        struct node* temp = graph->adjLists[v];
        printf("Vertex %d: ", v);
        while (temp) {
            printf("(%d, %d) -> ", temp->vertex, temp->weight);
            temp = temp->next;
        }
        printf("NULL\n");
    }
}

int main() {
    int vertices = 4;
    struct Graph* graph = createGraph(vertices);

    addEdge(graph, 0, 1, 5);
    addEdge(graph, 0, 2, 3);
    addEdge(graph, 1, 2, 2);
    addEdge(graph, 2, 3, 4);

    printGraph(graph);

    return 0;
}
*/

/* 12 Write a C program to implement BFS. */
#include <stdio.h>
#include <stdlib.h>

#define MAX_QUEUE_SIZE 100

struct node {
    int vertex;
    struct node* next;
};

struct Graph {
    int numVertices;
    struct node** adjLists;
    int* visited;
};

struct Queue {
    int items[MAX_QUEUE_SIZE];
    int front;
    int rear;
};

struct node* createNode(int v) {
    struct node* newNode = malloc(sizeof(struct node));
    newNode->vertex = v;
    newNode->next = NULL;
    return newNode;
}

struct Graph* createGraph(int vertices) {
    struct Graph* graph = malloc(sizeof(struct Graph));
    graph->numVertices = vertices;

    graph->adjLists = malloc(vertices * sizeof(struct node*));
    graph->visited = malloc(vertices * sizeof(int));

    int i;
    for (i = 0; i < vertices; i++) {
        graph->adjLists[i] = NULL;
        graph->visited[i] = 0;
    }

    return graph;
}

void addEdge(struct Graph* graph, int src, int dest) {
    struct node* newNode = createNode(dest);
    newNode->next = graph->adjLists[src];
    graph->adjLists[src] = newNode;
}

struct Queue* createQueue() {
    struct Queue* queue = malloc(sizeof(struct Queue));
    queue->front = -1;
    queue->rear = -1;
    return queue;
}

int isEmpty(struct Queue* queue) {
    if (queue->rear == -1) {
        return 1;
    } else {
        return 0;
    }
}

void enqueue(struct Queue* queue, int value) {
    if (queue->rear == MAX_QUEUE_SIZE - 1) {
        printf("Queue is full\n");
    } else {
        if (queue->front == -1) {
            queue->front = 0;
        }
        queue->rear++;
        queue->items[queue->rear] = value;
    }
}

int dequeue(struct Queue* queue) {
    int item;
    if (isEmpty(queue)) {
        printf("Queue is empty\n");
        item = -1;
    } else {
        item = queue->items[queue->front];
        queue->front++;
        if (queue->front > queue->rear) {
            queue->front = queue->rear = -1;
        }
    }
    return item;
}

void bfs(struct Graph* graph, int startVertex) {
    struct Queue* queue = createQueue();

    graph->visited[startVertex] = 1;
    enqueue(queue, startVertex);

    while (!isEmpty(queue)) {
        int currentVertex = dequeue(queue);
        printf("Visited %d\n", currentVertex);

        struct node* temp = graph->adjLists[currentVertex];

        while (temp) {
            int adjVertex = temp->vertex;

            if (graph->visited[adjVertex] == 0) {
                graph->visited[adjVertex] = 1;
                enqueue(queue, adjVertex);
            }
            temp = temp->next;
        }
    }
}

int main() {
    struct Graph* graph = createGraph(4);

    addEdge(graph, 0, 1);
    addEdge(graph, 0, 2);
    addEdge(graph, 1, 2);
    addEdge(graph, 2, 0);
    addEdge(graph, 2, 3);
    addEdge(graph, 3, 3);

    bfs(graph, 2);

    return 0;
}

