# Queue

## Questions

1. Shortest Subarray with Sum at Least K** https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/

```
deque<pair<long long,int>>dq;
int ans=INT_MAX;
long long prefix_sum=0;


dq.push_back({0,-1});


for(int i=0;i<nums.size();i++)
{
    prefix_sum+=nums[i];

    while(!dq.empty()&&prefix_sum<=dq.back().first)
    dq.pop_back();


    long checker= prefix_sum-k;


    while(!dq.empty()&&checker>=dq.front().first)
    {
        ans=min(ans,i-dq.front().second);
        dq.pop_front();
    }
    dq.push_back({prefix_sum,i});
}

if(ans==INT_MAX)
return -1;
return ans;
```

2. First non repeating char in a stream 
3. Circular Queue
```
  class MyCircularQueue {
    vector<int> arr;
    int front = -1, back = -1;
    int len;

public:
    MyCircularQueue(int k) {
        arr.resize(k); // Initialize array with size 'k'
        len = k;
    }

    bool enQueue(int value) {
        if (isFull()) {
            return false;
        }
        if (front == -1 && back == -1) {
            front = 0;
            back = 0;
        } else {
            back = (back + 1) % len;
        }
        arr[back] = value;
        return true;
    }

    bool deQueue() {
        if (isEmpty()) {
            return false;
        }
        if (front == back) {
            front = -1;
            back = -1;
        } else {
            front = (front + 1) % len;
        }
        return true;
    }

    int Front() {
        if (isEmpty()) { // If queue is empty, return -1
            return -1;
        }
        return arr[front];
    }

    int Rear() {
        if (isEmpty()) { // If queue is empty, return -1
            return -1;
        }
        return arr[back];
    }

    bool isEmpty() {
        if (front == -1 && back == -1) {
            return true; // Queue is empty
        }
        return false;
    }

    bool isFull() {
        if ((back + 1) % len == front) {
            return true;
        }
        return false;
    }
};
```
4. Moving avg from data stream
   ```
      int windowSize;
    int sum;
    queue<int> window;
    MovingAverage(int size) : windowSize(size), sum(0) {}

    double next(int val) {
       window.push(val);
        sum += val;

        
        if (window.size() > windowSize) {
            sum -= window.front();
            window.pop();
        }


        return (double)sum / window.size();
    }

   ```
  
