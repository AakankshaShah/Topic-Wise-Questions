# Binary Tree

## Theory 

## Questions 

1. Sorted list to BST 

  ```
    if (head == nullptr)
        return nullptr;

    if (head->next == nullptr)
        return new TreeNode(head->val);

    ListNode* slow = head;
    ListNode* fast = head;
    ListNode* slow_prev = nullptr;

    while (fast != nullptr && fast->next != nullptr) {
        slow_prev = slow;
        slow = slow->next;
        fast = fast->next->next;
    }

    // slow points to mid
    if (slow_prev != nullptr)
        slow_prev->next = nullptr;

    TreeNode* root = new TreeNode(slow->val);
    root->left = sortedListToBST(head);
    root->right = sortedListToBST(slow->next);

    return root;
 ```
    
    
         

