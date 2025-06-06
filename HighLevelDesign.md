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

![image](https://github.com/user-attachments/assets/d3ab553c-be42-41c7-ac0a-d108b3ed85c1)

![image](https://github.com/user-attachments/assets/907c28e8-04a6-4af7-b6d4-c60e1f05de76)
```
User uses the application through a browser or a mobile app
Block servers upload files to cloud storage. Block storage is a technology which allows you to split a big file in blocks and store the blocks in a backing storage. Dropbox, for example, stores blocks of size 4mb.
Cloud storage - a file split into multiple blocks is stored in cloud storage
Cold storage - used for storing inactive files, infrequently accessed.
Load balancer - evenly distributes requests among API servers.
API servers - responsible for anything other than uploading files. Authentication, user profile management, updating file metadata, etc.
Metadata database - stores metadata about files uploaded to cloud storage.
Metadata cache - some of the metadata is cached for fast retrieval.
Notification service - Publisher/subscriber system which notifies users when a file is updated/edited/removed so that they can pull the latest changes.
Offline backup queue - used to queue file changes for users who are offline so that they can pull them once they come back online.
```

![image](https://github.com/user-attachments/assets/9c30eefe-28bb-4415-a34b-f38002a9a640)



10. Uber
11. Tinder
12. Spotify
13. Bookmyshow
14. Goibibo/Skyscanner
15. Flipkart/E-commerce
<img width="1455" alt="image" src="https://github.com/user-attachments/assets/308213c6-8917-4276-9db1-a831dc69915b" />

16. Leetcode
17. Logging System

![image](https://github.com/user-attachments/assets/ae24e1d8-0f89-4d7d-b269-d193d30a236e)

![image](https://github.com/user-attachments/assets/f3fcf02c-f61b-4d6b-b83b-10a03bd7d6ff)

![image](https://github.com/user-attachments/assets/acd39aae-025f-4f9d-a30f-9e30c68c206b)

![image](https://github.com/user-attachments/assets/fcb78899-2be7-43e4-89e3-e4e3dc80e7a8)
```
For this solution, the metrics collector needs to maintain an up-to-date list of services and metrics endpoints. We can use Zookeeper or etcd for that purpose - service discovery.
```

<img width="1025" alt="image" src="https://github.com/user-attachments/assets/f0d1778c-1e0d-42fb-994b-9af00e863f03" />



![image](https://github.com/user-attachments/assets/006a7cdd-e787-4556-b18a-f600aed3b1a6)

18. Zoom
19. Google Pay/UPI
20. Nearby Friends
21. Google Maps
22. Ad Click Event aggregation
23. Airbnb
24. Real time Gaming Leaderboar
25. Stock Exchange
26. Proximity service

![image](https://github.com/user-attachments/assets/94c3011c-9d2d-41e3-81f3-1fa940c60177)
```
    The load balancer automatically distributes incoming traffic across multiple services. A company typically provides a single DNS entry point and internally routes API calls to appropriate services based on URL paths.
Location-based service (LBS) - read-heavy, stateless service, responsible for serving read requests for nearby businesses
Business service - supports CRUD operations on businesses.
Database cluster - stores business information and replicates it in order to scale reads. This leads to some inconsistency for LBS to read business information, which is not an issue for our use-case
Scalability of business service and LBS - since both services are stateless, we can easily scale them horizontally

```
```
SELECT business_id, latitude, longitude,
FROM business
WHERE (latitude BETWEEN {:my_lat} - radius AND {:my_lat} + radius) AND
      (longitude BETWEEN {:my_long} - radius AND {:my_long} + radius)
```
```
Geohash works similarly to the previous approach, but it recursively divides the world into smaller and smaller grids, where each two bits correspond to a single quadrant:
```

![image](https://github.com/user-attachments/assets/05d5724a-3c70-424a-bdef-b00d5dfe8e87)
```
Client tries to locate restaurants within 500meters of their location
Load balancer forwards the request to the LBS
LBS maps the radius to geohash with length 6
LBS calculates neighboring geohashes and adds them to the list
For each geohash, LBS calls the redis server to fetch corresponding business IDs. This can be done in parallel.
Finally, LBS hydrates the business ids, filters the result and returns it to the user
Business-related APIs are separated from the LBS into the business service, which checks the cache first for any read requests before consulting the database
Business updates are handled via a nightly job, which updates the geohash store
```
27. Video Streaming
     ```
          <img width="1554" alt="image" src="https://github.com/user-attachments/assets/be808610-c988-457b-82d8-bd9a8da95ede" />

     ```

    
