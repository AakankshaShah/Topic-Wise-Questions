https://github.com/presmihaylov/booknotes/tree/master/system-design/system-design-interview



1. Rate Limitter
2. URL Shortener
3. Web Crawler
![image](https://github.com/user-attachments/assets/00dad9aa-d720-453f-a802-15474c5a9d2f)
![image](https://github.com/user-attachments/assets/2931868a-9ff7-4c3c-b792-c1328e8191f3)


4. Notification System
![image](https://github.com/user-attachments/assets/0551f731-4e26-4d94-b733-4d96ac54f06c)
![image](https://github.com/user-attachments/assets/adc489f5-7659-425d-b7a1-214815e07ce7)


5. News Feed System (Instgaram, Facebook, Twitter)
![image](https://github.com/user-attachments/assets/4cc20d12-5bf4-4a5c-af9d-760f27c3948d)
![image](https://github.com/user-attachments/assets/d40d5b09-44f8-4f8f-be97-504d99ce6619)
![image](https://github.com/user-attachments/assets/c3712251-2d7c-4359-978b-fb6a712b1054)
![image](https://github.com/user-attachments/assets/a4a3007a-0b18-4ee7-9e8f-94260870635e)
![image](https://github.com/user-attachments/assets/5afb6c00-e702-4f34-9320-f6435ddaae35)





6. Chat System (1:1 and group)
```
When clients connect to the server, they can do it via one or more network protocols. One option is HTTP. That is okay for the sender-side, but not okay for receiver-side.

There are multiple options to handle a server-initiated message for the client - polling, long-polling, web sockets.
```
![image](https://github.com/user-attachments/assets/339ed31a-0cbe-4bc2-b357-0bc52b69afad)
![image](https://github.com/user-attachments/assets/0e6c8dea-183c-4300-a806-6e4de8af99fa)
![image](https://github.com/user-attachments/assets/be84b7d5-1b7b-4e7e-a056-c582f7d8391b)



7. Search Autocomplete system

<img width="1086" alt="image" src="https://github.com/user-attachments/assets/a1d7a42d-36ae-453a-80ce-016ce1eec45f" />

<img width="1069" alt="image" src="https://github.com/user-attachments/assets/9f968996-523e-4606-a44c-43a3df41471c" />

![image](https://github.com/user-attachments/assets/7885579a-b7c6-44c5-81f4-a10754e5e76d)

```
How to support multi-language - we store unicode characters in trie nodes, instead of ASCII.
What if top search queries differ across countries - we can build different tries per country and leverage CDNs to improve response time.
```


8. Youtube

![image](https://github.com/user-attachments/assets/ae84e308-8bb6-4bde-80a9-8611b5316c03)
```
Videos are uploaded to original storage
Transcoding servers fetch videos from storage and start transcoding
Once transcoding is complete, two steps are executed in parallel:
Transcoded videos are sent to transcoded storage and distributed to CDN
Transcoding completion events are queued in completion queue, workers pick up the events and update metadata database & cache
API servers inform user that uploading is complete
```

![image](https://github.com/user-attachments/assets/f58bcb51-ce66-4db6-a0e2-dcaece5e8037)


 

9. Google Drive
10. Uber
11. Tinder
12. Spotify
13. Bookmyshow
14. Goibibo/Skyscanner
15. Flipkart/E-commerce
16. Leetcode
17. Logging System
18. Zoom
19. Google Pay/UPI
20. Nearby Friends
21. Google Maps
22. Ad Click Event aggregation
23. Airbnb
24. Real time Gaming Leaderboar
25. Stock Exchange
