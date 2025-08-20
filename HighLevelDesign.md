

---

1. Scale upto 1 million

- **Database Scaling**  
  - Vertical scaling  
  - Horizontal scaling  
- **Load Balancer**
- **Database Replication**  
  - Master & Slave
- **Cache**
- **CDN**
- **Stateless Architecture**
- **Data Centre**
- **Message Queue**
- **Logging, Metrics, Automation**
<img width="641" height="781" alt="scaleupto1Million drawio" src="https://github.com/user-attachments/assets/7b95937d-10a5-4e8c-b0eb-49372ca28c65" />

---

2. Rate Limiter
- server side ( not client side as client requests can be easily forged )
- 429
- **Algo**
  - Token bucket
  - Leaky Bucket
  - Fixed window counter
  - Sliding window log
  - Sliding window counter
- In memory cache 
- Distributed issues
    - race : Locks
    - synchronisation : Sticky sessions and centralized redis 
<img width="778" height="587" alt="RateLimiter drawio" src="https://github.com/user-attachments/assets/32feee18-935e-4d75-82aa-acdfce078fb1" />

---
3. Key Value Store
- Scope
  - Size of key-value pair small
  - Ability to store big data
  - High scalability
  - High availibility
  - Automatic scaling
  - Tunable conistency
  - Low latency

Single server - hast table - cache - rest on disk - not feasible for large datasets 
    
- Components 
  - Data partition - Conistent hashing
  - Data replication - N servers replicate
  - Conistency - R+W > N Strong 
  - Inconsistency resolution  Versioning , vector clocks
  - Handling failures
     - Failures detection - Gossip protocol
     - Temporary - Sloppy Quorum
     - Permanent - Merkle tree
  - System architecture diagram
  - Write path
  - Read path

| Goal / Problem                     | Technique                                                   |
|-------------------------------------|-------------------------------------------------------------|
| Ability to store big data           | Use consistent hashing to spread load across servers        |
| High availability reads             | Data replication, Multi-datacenter setup                    |
| Highly available writes             | Versioning and conflict resolution with vector clocks       |
| Dataset partition                   | Consistent hashing                                          |
| Incremental scalability             | Consistent hashing                                          |
| Heterogeneity                       | Consistent hashing                                          |
| Tunable consistency                 | Quorum consensus                                            |
| Handling temporary failures         | Sloppy quorum and hinted handoff                            |
| Handling permanent failures         | Merkle tree                                                 |
| Handling data center outage         | Cross-datacenter replication                                |

<img width="781" height="381" alt="image" src="https://github.com/user-attachments/assets/198af5b9-3cee-499d-8d5d-05bb533b41fd" />

---
4. Uuid Generator
- Features
  - Ids must be unique
  - Ids are numerical values
  - 64 bit
  - Ordered by date
  - 10,000 ids per second
- Ways
   - Multi master replication : Uses auto increment by k ( no of servers)
   - Uuid :  128 bits , non numeric, do not increase with time 
   - Ticket Server : Centralized database auto increment , spof
   - Twitter snowflake: Sign Bit (1), Timestamp (41) , datacentreId(5), machineId(5), sequeneceNumber ( 12) 
     Issue - Clock sync 
Improve - More timestamp bits , less sequence 



---

5. URL Shortener
- 301 permanently moved , cached
- 302 better for analytics
- Hash _ SHA-1, MD5, CRC32
- Base 62
<img width="761" height="338" alt="urlshortner drawio" src="https://github.com/user-attachments/assets/c7fd85bc-49e6-4a19-b155-4524c4c1b932" />

---

6. Web Crawler
- Usage
   - Search engine indexing 
   - Web archiving 
   - Web mining
   - Web monitoring 
- Features
   - scalability
   - robustness
   - politeness
   - extensibility
- DFS vs BFS
- BFS does not consider priority - > Url frontier 
Buy in diagram : 
![image](https://github.com/user-attachments/assets/00dad9aa-d720-453f-a802-15474c5a9d2f)
- Front queue -> priortizer
- Back queue - > politeness
![WhatsApp Image 2025-08-13 at 11 08 27 PM](https://github.com/user-attachments/assets/c4bd0af4-aa2b-4271-a03d-371696590dfe)

- freshness
- robots exclusion protocol - robots.txt
- Problematic content
 - spider traps -> infinite loop
 - data noise
 - reduendant content 
- Performance optimisation
    - Distributed crawl
    - Cache DNS Resolver
    - Locality
     Short timeout
- Robustness
    - Conistent hashing
    - Save state
    - Exception handling
    - Data validation
---

7. Notification System
- Types
    - Mobile Push
    - SMS
    - Email
- IOS Provider( builds and sends notification service)  -> APNS(Remote service  to propagate notifications Apple push notification service )->IOS( Client )
- Android Provider -> FCM (Firebase cloud messaging ) - > Client
- SMS Provider->SMS service (eg twilio)-> Client
- Mail 
- One notification server - > spof ,hard to scale , performace bottleneck
- Steps 
    - Contact info gathering 

<img width="1156" height="783" alt="notificationsystem drawio" src="https://github.com/user-attachments/assets/0ba9b34f-8144-4724-9ab7-b36cb7c59354" />

![image](https://github.com/user-attachments/assets/adc489f5-7659-425d-b7a1-214815e07ce7)

- Reliability - Commit log when pushed in queue and picked up by workers
- authentication and rate limiting 

---


8. News Feed System (Instagram, Facebook, Twitter)
- Post service
- Fanout service
- Notification service 
- News feed building service 

- Fanout write ( hotkey too many friends , done for inactive )  vs read - on demand 
- Cache 
   - News feed
   - Content
   - Social Graph
   - Action
   - Counter 

![image](https://github.com/user-attachments/assets/4cc20d12-5bf4-4a5c-af9d-760f27c3948d)
![image](https://github.com/user-attachments/assets/d40d5b09-44f8-4f8f-be97-504d99ce6619)
<img width="1374" height="1518" alt="image" src="https://github.com/user-attachments/assets/ecca0dbc-ed66-4140-83c4-1a18ea5cc0e6" />

![image](https://github.com/user-attachments/assets/c3712251-2d7c-4359-978b-fb6a712b1054)



---


9. Chat System (1:1 and group)
```
When clients connect to the server, they can do it via one or more network protocols. One option is HTTP. That is okay for the sender-side, but not okay for receiver-side.

There are multiple options to handle a server-initiated message for the client - polling, long-polling, web sockets.
```
- HTTP - Client initiated
- Polling  -> not resourceful and inefficient
- Long polling -> client disconnected, sender and receiver not on same server , inefficient 
- Web sockets
    - HTTP Handshake
    - Acknowledgement 
    - Bidirectional messages
sender <-> ws -> chat server
reciever <-> ws -> chat server
- Stateless -> service directory , authentication service , group management , user profile 
- Stateful -> ws 
- Third party - Notification service
- 1: read/ write mostly
- KV for storage
   - easy horizontal scaling
   - low latency
   - rdbms - > random acess is expensive
  - Group - local sequnece number is enough as uuid 




![image](https://github.com/user-attachments/assets/339ed31a-0cbe-4bc2-b357-0bc52b69afad)
![image](https://github.com/user-attachments/assets/0e6c8dea-183c-4300-a806-6e4de8af99fa)
![image](https://github.com/user-attachments/assets/be84b7d5-1b7b-4e7e-a056-c582f7d8391b)

- 1:1 Chat 

<img width="886" height="496" alt="chatsystem drawio" src="https://github.com/user-attachments/assets/fe105b7c-f47b-4bce-926a-128b2e0a76e8" />
# Chat System Message Flow

## User A → Chat Server 1

-   User A sends a message.
-   The **ID Generator** assigns a unique message ID.
-   **Chat Server 1** forwards the message into the **Message Queue**
    and also writes metadata (delivery state, recipient mapping) into
    the **KV Store**.

## Message Queue

-   Acts as the transport backbone.
-   **Chat Server 1** publishes messages into the queue.
-   **Chat Server 2** subscribes to the queue to fetch messages for its
    connected users.

## KV Store

-   Stores message metadata (e.g., "delivered", "read", "pending").
-   Allows **Chat Server 2** to retrieve historical state or resume
    delivery if it missed something.

## Chat Server 2 (for User B)

-   Subscribes to the message queue.
-   Queries the **KV Store** if needed (for offline messages, sync, or
    state reconciliation).
-   If **User B** is **online**, Chat Server 2 delivers messages
    directly.
-   If **User B** is **offline**, the **Push Notification Service** is
    triggered.
- Chat Server 2 (the server handling User B’s session) subscribes only to messages relevant for its connected users.For example: If User B is connected to Chat Server 2, that server consumes messages where recipient = User B.Chat Server 2 ignores all other traffic in the queue.

## Push Notification Service

-   Wakes User B's client when offline.
-   Once User B comes online, **Chat Server 2** resyncs messages from
    the **KV Store**.


## 
- Message sync across devices - curr_max_messagea_id
- Group chat 
<img width="301" height="281" alt="groupchat drawio" src="https://github.com/user-attachments/assets/5ad46c19-389b-45f0-a75e-e4316457e74d" />



----

10. Search Autocomplete system
- Fast respone time
- Relevant
- Sorted
- Scalable
- Highly available

- Services
   - Data gathering sevrice
   - Query service
- Trie data structure O (p) + O(C) + O(clogc) , p = length , c= children
- Limit max length of prefix
- Cache top at eACh node
- AJAX REQUEST -> (Asynchronous JavaScript and XML) is a technique in web apps that lets the browser send a request to the server without reloading the page.
- Shard for each character
- unicode for multi lang 



<img width="1086" alt="image" src="https://github.com/user-attachments/assets/a1d7a42d-36ae-453a-80ce-016ce1eec45f" />

<img width="1069" alt="image" src="https://github.com/user-attachments/assets/9f968996-523e-4606-a44c-43a3df41471c" />

![image](https://github.com/user-attachments/assets/7885579a-b7c6-44c5-81f4-a10754e5e76d)

```
How to support multi-language - we store unicode characters in trie nodes, instead of ASCII.
What if top search queries differ across countries - we can build different tries per country and leverage CDNs to improve response time.
```

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/90a7bfaf-aed5-40b3-8826-48c92ff8b0c2" />


----

11. Youtube
https://www.youtube.com/watch?v=PuU_0esYyhg&t=48s
- ability to upload videos fast
- smooth video streaming 
- change video quality
- low infra cost 
- high availability , scalability and reliability 





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
- break upa nd store videos to original storage
- pre signed url for authorised user to upload
- drm digital rights management 

<img width="1350" height="789" alt="image" src="https://github.com/user-attachments/assets/55b62b48-98b5-44a9-abcc-6dffe8caeac9" />



 ---

12. Google Drive
- Reliability
- Fast sync speed
- Bandwidth usage
- Scalability
- High availability 

- Web server to download and upload
- Metadata db
- Storage sytem to store files
- Apis : Upload , download , get file revision
- All apis use user authentication and httpd , SSL (Secure Sockets layer) protect data transfer
  
<img width="775" height="714" alt="googledrive drawio" src="https://github.com/user-attachments/assets/c8307d01-019d-4908-adb0-3f01bcf587b8" />

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
- Block storage example - EBS ( elastic block storage , Provides persistent storage volumes for EC2 instances)
- Delta sync - only blocks updated
-  Sync Conflict - versioning 
- High conistency
    - Data in cache and master same 
    - Invalidate cache on database write hence RDBMS due to acid 

-  DB : User , Device , Namespace, File , Block 
- Why long polling : not bidirectional
- Limit on version saved , move to cold storage
- 
   
```
File split
Compressed
Encyrpted
Uploaded to cloud storage
```

![image](https://github.com/user-attachments/assets/907c28e8-04a6-4af7-b6d4-c60e1f05de76)
<img width="1582" height="878" alt="image" src="https://github.com/user-attachments/assets/90117a0e-0edb-4720-9d38-c722694b2ea1" />

![image](https://github.com/user-attachments/assets/9c30eefe-28bb-4415-a34b-f38002a9a640)

---
12. Proximity service

- FR
   - Return all business based on user's location
   - Business owners can add , delete and update
   - Customer can view detialed info about the business

- NFR
   - Low latency
   - Data privacy
   - Highly available and scalable

- API 
    - Get , request params - latitude,longitude,radius
    - Ouput is paginated : Pagination is the process of breaking a large dataset (e.g., millions of files, database rows, or API results) into smaller chunks (pages) so that              clients don’t need to load everything at once
     - Business : get , post , put , delete 

- Read & Write 
   - Ready heavy - rdbms better approach
- DB schema
<img width="740" height="870" alt="image" src="https://github.com/user-attachments/assets/5eb7cf3e-c695-455c-a0c8-8a5281f87f9d" />
- Geo index table - used for efficient processing of spatial operations , specifically geo locations

<img width="1480" height="1312" alt="image" src="https://github.com/user-attachments/assets/cb768abd-b5b9-4ee0-90f3-22336fb75781" />

- Algo 
       - Existing Geospatial db like Geohash in redis and postGress with PostGIS
       - 2d search - Draw a circle - not efficeint as searching entire table and indexing wont help much 
      ```
          SELECT business_id, lat,long
        FROM locations
       WHERE latitude BETWEEN (my_lat - radius) AND (my_lat + radius)
       AND longitude BETWEEN (my_long - radius AND (lmy_long - radius);
      ```
      - Hash and tree : Divide map into smaller ares and build indexes for fast search
      - Evenly divided grids as unequal division of business
      - Geo hash better approach : base 32
      - Geo hash length 4-6 for 20km radius 
      - boundary issues : 1. same longer prefix closer , but may be closer but no same perfix  2. same longer prefix but different geohash
      - Fetch all business not only within current grid but also neighbours
      - Not enough business - 1. return as found , 2,increase search radius
      - Quadtree
      - Google S2 - like quad tree maps sphere to 1d based on hilbert curve
      - Scaling - shard by business id
      - Geospatial index , compund key as geohash so easy to remove business rathen than geohash has json of business
       <img width="641" height="571" alt="proximityservice drawio" src="https://github.com/user-attachments/assets/dd2c1e95-ab45-4643-92d5-dc0d42703c66" />

---
13. Nearby friends

- FR
    - See nearby with distance & timestamp
    - Updated few seconds
      
- NFR
    - low latency
    - reliable
    - eventual consistency
      
- Design 
<img width="1624" height="1272" alt="image" src="https://github.com/user-attachments/assets/5af233d6-25b3-4759-9817-5379659a50c0" />
```
The load balancer spreads traffic across rest API servers as well as bidirectional web socket servers
The rest API servers handles auxiliary tasks such as managing friends, updating profiles, etc
The websocket servers are stateful servers, which forward location update requests to respective clients. It also manages seeding the mobile client with nearby friends locations at initialization (discussed in detail later).
Redis location cache is used to store most recent location data for each active user. There is a TTL set on each entry in the cache. When the TTL expires, user is no longer active and their data is removed from the cache.
User database stores user and friendship data. Either a relational or NoSQL database can be used for this purpose.
Location history database stores a history of user location data, not necessarily used directly within nearby friends feature, but instead used to track historical data for analytical purposes
Redis pubsub is used as a lightweight message bus which enables different topics for each user channel for location updates.
<img width="1598" height="726" alt="image" src="https://github.com/user-attachments/assets/d6f79e22-ce4f-4522-a280-939cfe807c2f" />
```

<img width="1598" height="726" alt="image" src="https://github.com/user-attachments/assets/28a61be7-f924-41af-8d4d-850c5f00c4e3" />

     

---
14. Google Maps
___
- FR
    - User location update
    - Navigation service
    - Map rendering
- NFR
    - accuracy
    - smooth navigation
    - data & battery usage
    - general availability and scalability
- Map Positioning 
     - Geocoding
     - Geohashing 
     - Map rendering : tiling
         - Hiearchical routing tiles
          
   
- Path finding algo 
    - Dijkstra
    - A's pathfinding algo 
- Storage
    - Map of the world
    - Metadata
    - Road data
    - Zoom level 21 , 4.3 trillion tiles , 256*256 pixels , 100 kb , 440PB--> reduce as most area is hills,oceans etc -> 50PB
    - all levels around 100PB

    <img width="1362" height="872" alt="image" src="https://github.com/user-attachments/assets/07ce5c99-4386-4e7d-8e7d-0ff3994fd45a" />

    ```
       User opens app → Base map tiles fetched from CDN.

       User searches for an address → Request goes through Load Balancer → Navigation Service → Geocoding DB.

       User requests directions → Navigation Service fetches data from Routing Tiles, computes path.

       User shares live location → Location Service stores updates in User Location DB.
       Navigation service loads only the necessary tiles from object storage (instead of the entire map).Builds a subgraph connecting start tile → neighboring tiles → destination         tile.On the subgraph, run routing algorithms:

       Dijkstra’s Algorithm: Finds the shortest path (but slow for large networks).

       A*: Optimized Dijkstra with a heuristic (usually straight-line "Haversine distance" between nodes).

      Contraction Hierarchies (CH): Preprocess the graph by building shortcuts for fast long-distance queries (used by Google Maps).

      ALT (A* with Landmarks + Triangle Inequality) for further speed.
    ```

   - Send location update in a buffered batch way
   - High write volume , cassandra , kafka , http
   - Map rendering - not feasible to store entire map tiles of all zoom levels , use cdn
   - How to get tiles - based on goe hash 
<img width="1496" height="1416" alt="image" src="https://github.com/user-attachments/assets/4459fea4-ad8b-4817-8bc2-3fc7dbd71e4b" />
```
1. Mobile User

User requests directions (start → destination).

Sends the request to the Navigation Service.

2. Navigation Service

Acts as the orchestrator.

Breaks the request into subtasks:

Get coordinates of addresses (via Geocoding Service).

Ask Route Planner Service to compute route.

Collect results (route path, ETA, alternatives) and send back to the user.
3. Geocoding Service

Converts user’s start and destination addresses into latitude/longitude.

Uses Geocoding DB (mapping of addresses, POIs, landmarks → coordinates).

Sends results back to Navigation Service.
This is the core of directions computation.
It coordinates three sub-services:

a) Shortest Path Service

Responsible for computing the actual path on the road graph.

Loads Routing Tiles (object storage) containing the road network split into tiles (nodes = intersections, edges = roads).

Runs algorithms like:

A* (fast search with heuristics),

Contraction Hierarchies (CH) (for precomputed shortcuts),

Dijkstra (baseline).

Returns one or multiple candidate routes.

b) ETA Service

Computes Estimated Time of Arrival for each candidate route.

Uses Traffic DB (historical + real-time speeds).

Update Traffic Service pushes live data from Location DB (crowdsourced user GPS, sensors, reports).

Adjusts road speeds, delays, closures.

c) Ranker

Receives candidate routes from Shortest Path Service.

Adjusts them based on:

ETA (from ETA Service),

Filters (from Filter Service: e.g., avoid tolls, ferries, highways),

User preferences (fastest, shortest, scenic).

Produces a final ranked list of routes.

5. Filter Service

Applies constraints like:

Avoid tolls,

Avoid highways,

Prefer bike lanes,

Truck restrictions (height/weight limits).

Works closely with Ranker.

6. Final Response

Navigation Service collects:

Best route polyline (from Shortest Path Service),

ETA (from ETA Service),

Alternative ranked routes (from Ranker).

Returns them to the Mobile User as JSON → rendered on map tiles.
```




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

    
