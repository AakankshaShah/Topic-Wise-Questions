# Two pointer approach 

## Questions 

1. is Subsequence

   ```
    int n = s1.length(), m = s2.length();
    int i = 0, j = 0;
    while (i < n && j < m) {
        if (s1[i] == s2[j])
            i++;
        j++;
    ```
