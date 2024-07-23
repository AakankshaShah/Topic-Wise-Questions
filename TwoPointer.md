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
2. Sum of square numbers
   ```
     bool judgeSquareSum(int c) {
        if (c < 0) return false;
        int l=0;
        int r=int(sqrt(c));
        
        while(l<=r)
        {
            int s=c-l*l;
            int sq=(int)sqrt(s);
            if(sq*sq==s)
            return true;
            l++;
        }
        return false;
        
    }
   ```
  ```
   bool judgeSquareSum(int c) {
        ll left = 0, right = static_cast<ll>(sqrt(c));
        while(left <= right) {
            if(left * left + right * right == c) return true;
            else if(left * left + right * right > c) right--;
            else left++;
        }

        return false;
    }
  ```
