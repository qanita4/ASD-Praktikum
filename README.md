#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next, *prev;
} Node;

Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = newNode->prev = newNode;
    return newNode;
}

void insertLast(Node** head, int data) {
    if (*head == NULL) {
        *head = createNode(data);
        return;
    }
    Node *end = (*head)->prev;
    Node* newNode = createNode(data);
    newNode->next = *head;
    (*head)->prev = newNode;
    newNode->prev = end;
    end->next = newNode;
}

void printList(Node* head) {
    if (head == NULL) {
        printf("Linked list kosong\n");
        return;
    }
    Node *curr = head;
    do {
        printf("Address: %p, Data: %d\n", (void*)curr, curr->data);
        curr = curr->next;
    } while (curr != head);
}

void sortList(Node **head) {
    if (*head == NULL) {
        printf("Linked list kosong\n");
        return;
    }

    int count = 0;
    Node *temp = *head;
    do {
        count++;
        temp = temp->next;
    } while (temp != *head);

    Node **nodeArray = (Node **)malloc(count * sizeof(Node *));
    temp = *head;
    for (int i = 0; i < count; i++) {
        nodeArray[i] = temp;
        temp = temp->next;
    }

    for (int i = 0; i < count - 1; i++) {
        for (int j = 0; j < count - i - 1; j++) {
            if (nodeArray[j]->data > nodeArray[j + 1]->data) {
                Node *tempNode = nodeArray[j];
                nodeArray[j] = nodeArray[j + 1];
                nodeArray[j + 1] = tempNode;
            }
        }
    }

    for (int i = 0; i < count; i++) {
        nodeArray[i]->next = nodeArray[(i + 1) % count];
        nodeArray[(i + 1) % count]->prev = nodeArray[i];
    }
    nodeArray[0]->prev = nodeArray[count - 1];
    nodeArray[count - 1]->next = nodeArray[0];

    *head = nodeArray[0];
    free(nodeArray);
}

int main() {
    int x, data;
    Node *head = NULL;
    printf("Masukkan jumlah data: "); 
    scanf("%d", &x);

    for (int i = 0; i < x; i++) {
        printf("Masukkan data ke-%d: ", i + 1);
        scanf("%d", &data);
        insertLast(&head, data);
    }

    printf("\nList sebelum pengurutan:\n");
    printList(head);
    sortList(&head);
    printf("\nList setelah pengurutan:\n");
    printList(head);
    return 0;
}
