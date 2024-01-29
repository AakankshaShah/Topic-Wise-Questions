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
