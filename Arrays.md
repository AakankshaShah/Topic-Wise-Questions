# Arrays

## Questions

1. Circular tour
2.  Maximum non negative subarray sum
     ```
     vector<int> result;
    long long sum = 0;
    int count = 0;
    int maxcount = 0;
    long long  maxsum = 0;
    int index = 0;
    for(int i = 0; i < A.size(); i++)
    {
        if(A[i] >= 0)
        {
            count++;
            sum = sum + A[i];
        }
        else
        {
            count = 0;
            sum = 0;
        }
        if(sum >= maxsum && maxsum != 0 || count >= maxcount && sum >= maxsum)
        {
            maxcount = count;
            maxsum = sum;
            index = i;
        }
    }

    for(int i = index - maxcount+1; i <=index ; i++)
    {
        result.push_back(A[i]);
    }
    return result;
    
     }

     ```
   3. Factorial of a large number https://www.youtube.com/watch?v=O3fwYjcMV_M

      ```
     void multiplier( vector<int> &arr, int &size,int fact)
     { void carry=0;
    for (int i=0;i<size;i++)
    {
        int res=arr[i]*fact;
        res=res+carry;
        arr[i]=res%10;
        carry=res/10;
    }
    
    while(carry>0)
    {
        arr[size++]=carry%10;
        carry=carry/10;
    }
    
     }
    vector<int> factorial(int N){
    
        vector<int> arr(10000,0);
        vector<int> res;
        arr[0] =1;
        int size=1;
        
        
        for(int fact=2;fact<=N;fact++)
        {
            multiplier(arr,size,fact);
        }
        
        
        for(int i=size-1;i>=0;i--)
        {
            res.push_back(arr[i]);
        }
        return res;

        
      }
      ```
4. Spiral order matrix traversal 
    ```
       int n=A.size();
    int m=A[0].size();
    int left=0;
    int right=m-1;
    int top=0;
    int bottom=n-1;
    vector<int>arr;
    
    while(left<=right&&top<=bottom)
    {
        for(int i=left;i<=right;i++)
        arr.push_back(A[top][i]);
        top++;
        
        for(int i=top;i<=bottom;i++)
        arr.push_back(A[i][right]);
        right--;
        
        for(int i=right;i>=left;i--)
        arr.push_back(A[bottom][i]);
        bottom--;
        
        for(int i=bottom;i>=top;i--)
        arr.push_back(A[i][left]);
        left++;
        
        
    }
    return arr;
    ```

