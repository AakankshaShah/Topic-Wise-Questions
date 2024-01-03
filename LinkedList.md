1. Add two numbers
2. Remove nth node from end (K-N+1) two pointer approach
3.  Reverse linked list 
     ```
      Node* current = head;
        Node *prev = NULL, *next = NULL;
 
        while (current != NULL) {
            // Store next
            next = current->next;
            // Reverse current node's pointer
            current->next = prev;
            // Move pointers one position ahead.
            prev = current;
            current = next;
        }
        head = prev;

      ```
4. Merge two sorted linked list
5. Merge k sorted linked lists
