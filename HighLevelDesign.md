
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

---
15. DISTRIBUTED MESSAGE QUEUE

FR
- Producer sends messages to a queue
- Consumers consume
- Consumed repeatedly / only once
- Historical data can be truncated
- Size in kb
- ability to deliver messages to consumer in order they added
- Data delivery semantics configurable
  

NFR
- high throughput / low latency
- scalable
- persistent & durable 


<img width="1726" height="216" alt="image" src="https://github.com/user-attachments/assets/879758f7-6ec8-4595-b621-17c6b4ffbf56" />

Messaging models
- Point to point 
- Pub/sub 
     - Messages are persisted by topics
     - Message consumption order -single partition by one consumer from consumer group 

| Term          | Simple Meaning                                   | Analogy (Post Office)                            | Key Point                                             |
| ------------- | ------------------------------------------------ | ------------------------------------------------ | ----------------------------------------------------- |
| **Topic**     | Logical channel/category for messages            | Newspaper title (e.g., “Sports News”)            | Groups related messages                               |
| **Partition** | Sub-division of a topic, ordered log of messages | Section of newspaper (football, cricket, tennis) | Keeps order **within partition**                      |
| **Broker**    | Server that stores partitions                    | Printing press building                          | Cluster has many brokers, each stores some partitions |
| **Offset**    | Position of a message inside a partition         | Page number in a newspaper section               | Lets consumers track where they left off              |


<img width="1716" height="794" alt="image" src="https://github.com/user-attachments/assets/4a37aea8-cb19-4b75-8e0c-d0c61e84d09a" />

- In order to achieve high throughput and preserve the high data retention requirement, we made some important design choices:

We chose an on-disk data structure which takes advantage of the properties of modern HDD and disk caching strategies of modern OS-es.
The message data structure is immutable to avoid extra copying, which we want to avoid in a high volume/high traffic system.
We designed our writes around batching as small I/O is an enemy of high throughput.

- Data storage
In order to find the best data store for messages, we must examine a message's properties:

Write-heavy, read-heavy
No update/delete operations. In traditional message queues, there is a "delete" operation as messages are not retained.
Predominantly sequential read/write access pattern.
What are our options:

Database - not ideal as typical databases don't support well both write and read heavy systems.
Write-ahead log (WAL) - a plain text file which only supports appending to it and is very HDD-friendly.
We split partitions into segments to avoid maintaining a very large file.
Old segments are read-only. Writes are accepted by latest segment only.

- Batching
Batching is critical for the performance of our system. We apply it in the producer, consumer and message queue.

It is critical because:

It allows the operating system to group messages together, amortizing the cost of expensive network round trips
Messages are written to the WAL in groups sequentially, which leads to a lot of sequential writes and disk caching.

- Producer flow 
<img width="1204" height="838" alt="image" src="https://github.com/user-attachments/assets/ddf2d7bc-78d8-4461-949d-d053ee45f682" />
<img width="1346" height="894" alt="image" src="https://github.com/user-attachments/assets/17aae196-40fd-411f-968d-04aa76922194" />

- Consumer flow 
The consumer specifies its offset in a partition and receives a chunk of messages, beginning from that offset:
<img width="1718" height="488" alt="image" src="https://github.com/user-attachments/assets/5b3f738a-4cce-4c41-802e-ab0cb448bf82" />

- Push model leads to lower latency as broker pushes messages to consumer as it receives them.
However, if rate of consumption falls behind the rate of production, the consumer can be overwhelmed.
It is challenging to deal with consumers with varying processing power as the broker controls the rate of consumption.

Pull model leads to the consumer controlling the consumption rate.
If rate of consumption is slow, consumer will not be overwhelmed and we can scale it to catch up.
The pull model is more suitable for batch processing, because with the push model, the broker can't know how many messages a consumer can handle.
With the pull model, on the other hand, consumers can aggressively fetch large message batches.
The down side is the higher latency and extra network calls when there are no new messages. Latter issue can be mitigated using long polling.
Hence, most message queues (and us) choose the pull model.

- Consumer rebalancing
Consumer rebalancing is responsible for deciding which consumers are responsible for which partition.

<img width="1740" height="730" alt="image" src="https://github.com/user-attachments/assets/ad82a4e9-276a-4569-9a86-c4ba0d5c381b" />

- One problem we need to tackle is keeping messages in-sync between the leader and the followers for a given partition.

In-sync replicas (ISR) are replicas for a partition that stay in-sync with the leader.

The replica.lag.max.messages defines how many messages can a replica be lagging behind the leader to be considered in-sync.

- Data delivery semantics
     - At most once : ACK =0 
     - At least once : ACK 1/ACK=ALL
     - Extremely costly to implement for the system, albeit it's the friendliest guarantee to users

- Temp storage for message schedule
<img width="1346" height="894" alt="image" src="https://github.com/user-attachments/assets/d0547d77-0c71-42f9-a9af-43271f3f356d" />
---

16. Metrics monitoring and alert system 
- BOE
   - 100 metrics per machine , 100 machine per pool, 1000 server pools
   - 10^7 logs 

- NFR 
   - Scalability
   - Low latency
   - Reliabiility
   - Flexibility
![image](https://github.com/user-attachments/assets/ae24e1d8-0f89-4d7d-b269-d193d30a236e)

- Data model
   - Metrics data usually recorded  as time series contains sets of data with their associated timestamps
   - metric name , tag (key value pair) , timestamps array 
- Data access pattern - write heavy and read spiky
- Data storage system
   - use tiemseries db 
  
![image](https://github.com/user-attachments/assets/f3fcf02c-f61b-4d6b-b83b-10a03bd7d6ff)

- Pull vs Push
<img width="1398" height="772" alt="image" src="https://github.com/user-attachments/assets/4b2d1d3a-a460-4ead-bcc1-790b9df394a3" />

```
For this solution, the metrics collector needs to maintain an up-to-date list of services and metrics endpoints. We can use Zookeeper or etcd for that purpose - service discovery.
```

<img width="1025" alt="image" src="https://github.com/user-attachments/assets/f0d1778c-1e0d-42fb-994b-9af00e863f03" />
<img width="1714" height="590" alt="image" src="https://github.com/user-attachments/assets/531b88d1-5a4a-4c87-9689-d220ba185cdd" />

```
With this model, we can potentially aggregate metrics before sending them to the collector, which reduces the volume of data processed by the collector.
On the flip side, metrics collector can reject push requests as it can't handle the load. It is important, hence, to add the collector to an auto-scaling group behind a load balancer.
```
<img width="1126" height="725" alt="image" src="https://github.com/user-attachments/assets/f5dd3596-f9b2-4820-8e46-ce7d3e38cdbb" />
![image](https://github.com/user-attachments/assets/006a7cdd-e787-4556-b18a-f600aed3b1a6)

- There is a chance of data loss if the time-series DB is down, however. To mitigate this, we'll provision a queuing mechanism:
<img width="1866" height="714" alt="image" src="https://github.com/user-attachments/assets/7d4e67ee-e77c-4b7c-a047-31675afecb33" />

- This approach has several advantages:

Kafka is used as a highly-reliable and scalable distributed message platform
It decouples data collection and data processing from one another
It can prevent data loss by retaining the data in Kafka

Kafka can be configured with one partition per metric name, so that consumers can aggregate data by metric names. To scale this, we can further partition by tags/labels and categorize/prioritize metrics to be collected first.

- We can add a Cache layer here to reduce the load to the time-series database:
<img width="1942" height="904" alt="image" src="https://github.com/user-attachments/assets/c51baf5e-0623-4ade-9118-abd5c1697ad8" />

- Alerting system
<img width="1956" height="708" alt="image" src="https://github.com/user-attachments/assets/ad507073-c1bb-4d99-a285-dce0c469e881" />

- Cold storage of inactive data
---
17. Ad cLick event aggregation



Digital advertising has a process called real-time bidding (RTB), where digital advertising inventory is bought and sold:<img width="1930" height="244" alt="image" src="https://github.com/user-attachments/assets/b03e21cf-7bd6-434f-a9e7-184553f09bd7" />

- FR
   - Aggregate no ad_id clicks in last M minutes
   - Return 100 most clicked ads
   - Support aggregation filtering by diff attributes

- NFR
   - Correctness
   -  Handle delay/ duplicate
   -  Robusntness
   -  Few minutes latency
- API
   - get ads/ad_id/aggregated_count
   - get ads/popular_ads
     
- Data model 
<img width="686" height="216" alt="image" src="https://github.com/user-attachments/assets/fcd4a860-3ccd-455b-993e-eec890e2aad6" />
<img width="559" height="414" alt="image" src="https://github.com/user-attachments/assets/3b131559-ce65-4772-a15e-e312fc2f3540" />
<img width="840" height="226" alt="image" src="https://github.com/user-attachments/assets/0c7f9c6a-f440-49ae-a1bd-488477bd44c8" />

- Cassandra- have better native support for heavy write loads
<img width="1812" height="400" alt="image" src="https://github.com/user-attachments/assets/9ddb8626-ee5e-42dd-8534-3fd60c938569" />
  - Async 
<img width="1778" height="1024" alt="image" src="https://github.com/user-attachments/assets/d92f7ebe-ce31-483d-8a31-6bf7704194c8" />

```
The first message queue stores ad click event data:

ad_id	click_timestamp	user_id	ip	country

The second message queue contains ad click counts, aggregated per-minute:

ad_id	click_minute	count
As well as top N clicked ads aggregated per minute:

update_time_minute	most_clicked_ads
The second message queue is there in order to achieve end to end exactly-once atomic commit semantics
```
- Aggregation service 
    - map reduce , dag 
<img width="1160" height="804" alt="image" src="https://github.com/user-attachments/assets/49b55d56-0673-4250-9312-2f4ba0db9a54" />
<img width="1570" height="822" alt="image" src="https://github.com/user-attachments/assets/8e9e1f75-b461-459c-8a00-0473cee13b92" />
- Map , aggregate node , reduce
- The aggregate node counts ad click events by ad_id in-memory every minute.
- Use-case 1 - aggregate the number of clicks:
<img width="1856" height="758" alt="image" src="https://github.com/user-attachments/assets/8484e138-ad80-448d-8e6b-f2563a7eb41c" />
- Use-case 2 - return top N most clicked ads:
<img width="1844" height="1030" alt="image" src="https://github.com/user-attachments/assets/33a4ee6e-0fee-4fe3-8ed8-fb576a83292d" />

- Use-case 3 - data filtering:
     - To support fast data filtering, we can predefine filtering criterias and pre-aggregate based on it
     - This technique is called the star schema and is widely used in data warehouses. The filtering fields are called dimensions.
     - This approach has the following benefits:
          - Simple to undertand and build
          - Current aggregation service can be reused to create more dimensions in the star schema.
          - Accessing data based on filtering criteria is fast as results are pre-calculated

    - A limitation of this approach is that it creates many more buckets and records, especially when we have lots of filtering criterias.
      
- <img width="1106" height="405" alt="image" src="https://github.com/user-attachments/assets/e5a1ee36-c94a-4018-8e4b-5d938118fcc3" />

- In our design, we used a mixture of batching and streaming.

We used streaming for processing data as it arrives and generates aggregated results in near real-time. We used batching, on the other hand, for historical data backup.

A system which contains two processing paths - batch and streaming, simultaneously, this architecture is called lambda. A disadvantage is that you have two processing paths with two different codebases to maintain.

Kappa is an alternative architecture, which combines batch and stream processing in one processing path. The key idea is to use a single stream processing engine.
<img width="1868" height="716" alt="image" src="https://github.com/user-attachments/assets/b3bc014f-615e-452e-abc7-f4d213830f76" />
<img width="1860" height="540" alt="image" src="https://github.com/user-attachments/assets/3af0edd8-730e-4e8c-b56e-f36cf963471e" />
<img width="1940" height="692" alt="image" src="https://github.com/user-attachments/assets/7d4c492f-b4b6-43d3-adf1-9e544daa9f64" />

- We need timestamp for aggregation
<img width="1065" height="177" alt="image" src="https://github.com/user-attachments/assets/f0de9191-4b0f-4f6c-9973-c9e05f12e30d" />
- However, if we purposefully extend the aggregation window, we can reduce the likelihood of missed events. The extended part of a window is called a "watermark":
    - Short watermark increases likelihood of missed events, but reduces latency
    - Longer watermark reduces likelihood of missed events, but increases latency
<img width="1950" height="606" alt="image" src="https://github.com/user-attachments/assets/d5902e9d-00cd-45dc-96ab-b741edf66f4d" />

- There are four types of window functions:

Tumbling (fixed) window
Hopping window
Sliding window
Session window

- We'll need to use exactly-once delivery semantics.
- Use S3 to record offset
- 

---
18. Hotel Reservation
---
19. Distributed mail system 

- FR
    - Authetication
    - Send and receive mails
    - Fetch all mails
    - Filter mails
    - Search mails
    - Anti spam and anti virus

- HTTP used for client and server communication, traditionally smtp,pop,imap etc.
- SMTP (Simple Mail Transfer Protocol) -Purpose: Sending emails
- POP (Post Office Protocol, usually POP3) -Purpose: Receiving emails (download-and-delete model) , deletes once read from server
- IMAP (Internet Message Access Protocol) - Purpose: Receiving emails (sync model) , not deleted
- HTTPS - Outlook
- 

- NFR
    - reliable
    - scalable
    - available
    - flexible and extensible 

<img width="1166" height="928" alt="image" src="https://github.com/user-attachments/assets/6b49267e-bbd4-475b-8c18-1e60565753b7" />

- Maildir was a popular way to store mails on mail server.
- APIS
    - post :  send mail
    - get : all folders of an mail account
    - get : all messages in a folder
    - get : specifics about a mail

<img width="1328" height="1040" alt="image" src="https://github.com/user-attachments/assets/541684c0-9c60-4bab-aced-3a4c572b80c4" />

- Webmail - users use web browsers to send/receive emails
- Web servers - public-facing request/response services used to manage login, signup, user profile, etc.
- Real-time servers - Used for pushing new email updates to clients in real-time. We use websockets for real-time communication but fallback to long-polling for older browsers that don't support them.
- Metadata db - stores email metadata such as subject, body, from, to, etc.
- Attachment store - Object store (eg Amazon S3), suitable for storing large files (S3)
- Distributed cache - We can cache recent emails in Redis to improve UX.
- Search store - distributed document store, used for supporting full-text searches , uses data structure called inverted index
- Inverted index (for search engines):

```
Each term (word) maps to a list of documents (emails) containing that term.
Example:
"project" → [EmailID: 101, 305, 420]  
"meeting" → [EmailID: 101, 222, 333, 420]  
Eg : Elasticstore 
```

<img width="1698" height="1126" alt="image" src="https://github.com/user-attachments/assets/0f285cd2-76ba-4419-9b83-87528f5b7440" />

- Mail sending flow 
- User writes an email and presses "send". Email is sent to load balancer.
- Load balancer rate limits excessive mail sends and routes to one of the web servers.
- Web servers do basic email validation (eg email size) and short-circuits outbound flow if domain is same as sender. But does spam check first.
- If basic validation passes, email is sent to message queue (attachment is referenced from object store)
- If basic validation fails, email is sent to error queue
- SMTP outgoing workers pull messages from outgoing queue, do spam/virus checks and route to destination mail server.
- Email is stored in the "Sent Emails" folder

<img width="1824" height="946" alt="image" src="https://github.com/user-attachments/assets/fdc1fbfb-4676-4c5e-b6d0-eada4d959b9d" />

- Mail receiving flow
- Incoming emails arrive at the SMTP load balancer. Mails are distributed to SMTP servers, where mail acceptance policy is done (eg invalid emails are directly discarded).
 - If attachment of email is too large, we can put it in object store (s3).
 - Put in incoming mail queue
 - Mail processing workers - filtering , spam , virus
 - mail stored in mail storage , cache , object data store
 - online - pushed to real time server
 - offline - storage layer - served via rest api
   

- DB :
- Relational database - we can build indexes for headers and body, but these DBs are typically optimized for small chunks of data.
- Distributed object store - this can be a good option for backup storage, but can't efficiently support searching/marking as read/etc.
- NoSQL - Google BigTable is used by gmail, but it's not open-sourced.
- Based on above analysis, very few existing solutions seems to fit our needs perfectly. In an interview setting, it's infeasible to design a new distributed database solution, but important to mention characteristics:

- Single column can be a single-digit MB
- Strong data consistency
- Designed to reduce disk I/O
- Highly available and fault tolerant
- Should be easy to create incremental backups


- Search
- Elasticsearch
<img width="1210" height="1224" alt="image" src="https://github.com/user-attachments/assets/d669d2b5-294d-400e-b02c-dde5cf43922b" />
- Custom search
- To achieve that, we can use Log-Structured Merge-Trees (LSM) to structure the index data on disk. Write path is optimized for sequential writes only. This technique is used in Cassandra, BigTable and RocksDB.Its core idea is to store data in-memory until a predefined threshold is reached, after which it is merged in the next layer (disk).
  <img width="1926" height="1002" alt="image" src="https://github.com/user-attachments/assets/fe781206-d6e3-4364-a2f5-2680ef1378cb" />
---
20. S3 Storage
- Storage systems fall into three broad categories:

    - Block storage
    - File storage
    - Object storage - Stores all data as object in a flat structure 
- Terminology 
      - Bucket - logical container for objects. Name is globally unique.
      - Object - An individual piece of data, stored in a bucket. Contains object data and metadata.
      - Versioning - A feature keeping multiple variants of an object in the same bucket.
      - Uniform Resource Identifier (URI) - each resource is uniquely identified by a URI.
      - Service-level Agreement (SLA) - contract between service provider and client.

- NFR
   - 100 PB of data per year
   - Durability 6 nine
   - SLA 4 nine
   - Storage efficiency

- Bottlenecks at disk capacity / disk i/o.
- we have 20% small (less than 1mb), 60% mid-size (1-64mb) and 20% large objects (greater than 64mb),
- One hard disk (SATA, 7200rpm) is capable of doing 100-150 random seeks per second (100-150 IOPS) - Input/Output Operations Per Second
- 100PB*0.4/(0.2*0.5+0.4*32+0.2*200) = 0.68 billion objects
- metadata 1 kb then 0.68 TB space 

- Immutable objects
 - Key value store , uri as key , object as value
 - write once , read many times
   
<img width="1368" height="1058" alt="image" src="https://github.com/user-attachments/assets/d859871b-fbad-427a-9eb1-8cac1ef7ecd6" />


- Design philosophy of object storage is similar to UNIX - when we save a file, it creates the filename in a data structure, called inode and file data is stored in different disk locations. The inode contains a list of file block pointers, which point to different locations on disk.

- When accessing a file, we first fetch its metadata from the inode, prior to fetching the file contents.

- Object storage works similarly - metadata store is used for file information, but contents are stored on disk
- Inode becomes metastore
- Hard disk becomes data store

<img width="1596" height="1102" alt="image" src="https://github.com/user-attachments/assets/57aae0bd-83db-4b26-9e78-a3554248fd63" />

- Uploading an object
<img width="1638" height="1110" alt="image" src="https://github.com/user-attachments/assets/f6a1d853-bdf8-46d4-b04a-a8aa0d4dba1c" />

- Downloading an object 

<img width="1678" height="1130" alt="image" src="https://github.com/user-attachments/assets/3e9ab06c-90a2-4aa2-908f-09c98d247ddb" />

- Data store

<img width="1562" height="812" alt="image" src="https://github.com/user-attachments/assets/afc073e4-2b57-4771-924f-4932df5efa3a" />

<img width="1562" height="926" alt="image" src="https://github.com/user-attachments/assets/b7023ab9-125d-4ac4-aa28-f9c547162853" />
- The data routing service provides a RESTful or gRPC API to access the data node cluster. It is a stateless service, which scales by adding more servers.

- It's main responsibilities are:

  - querying the placement service to get the best data node to store data
  - reading data from data nodes and returning it to the API service
  - Writing data to data nodes

- The Data Store is made up of many Data Nodes (each is usually a physical server with local disks/SSDs).

- These issues can be addressed by merging many small files into bigger ones via a write-ahead log (WAL). Once the file reaches its capacity (typically a few GB), a new file is created

- To support storing multiple objects in the same file, we need to maintain a table, which tells the data node:

    - object_id
    - filename where object is stored
    - file_offset where object starts
    - object_size
      
- We can deploy this table in a file-based db like RocksDB or a traditional relational database. Since the access pattern is low write+high read, a relational database works better.

- <img width="1824" height="1032" alt="image" src="https://github.com/user-attachments/assets/3bf946d1-f642-4668-811d-8a0416bf0c0c" />
- Versioning works by having another object_version column which is of type TIMEUUID, enabling us to sort records based on it.

- <img width="1844" height="858" alt="image" src="https://github.com/user-attachments/assets/83225673-afca-4f44-9b1a-14dc3f622fc1" />

-<img width="1850" height="896" alt="image" src="https://github.com/user-attachments/assets/b0740f2c-8009-412e-9e42-ed349a17a464" />

---
21. Real time gaming leaderboard
- FR
   - Display top 10
   - show a user specific rank
   - Display 4 users above and below desired user

- NFR
   - real time update
   - score update reflected in real time
   - scalable, available and reliable

- BOE
     - With 5 mil DAU, if the game has an even distribution of players during a 24h period, we'd have an average of 50 users per second. However, since distribution is typically         uneven, we can estimate that the peak online users would be 250 users per second.

     - QPS for users scoring a point - given 10 games per day on average, 50 users/s * 10 = 500 QPS. Peak QPS = 2500.

     - QPS for fetching the top 10 leaderboard - assuming users open that once a day on average, QPS is 50.

- API
    - POST /v1/scores
    - GET /v1/scores
    - GET /v1/scores/{:user_id}

<img width="1102" height="1066" alt="image" src="https://github.com/user-attachments/assets/473253fa-d14d-42dd-94d5-5e3e63f227d4" />

- Data model
   - RDBMS - Good fr small set , we can index and limit
   - Redis - sorted sets / skip list - memory more than enough and for presistence read replica
   - 


<img width="1836" height="770" alt="image" src="https://github.com/user-attachments/assets/cf65f584-7f35-4ebc-9468-dca92d91e6a3" />

<img width="1978" height="496" alt="image" src="https://github.com/user-attachments/assets/cf61d2de-acf2-471f-8574-83fef885796c" />

<img width="1964" height="506" alt="image" src="https://github.com/user-attachments/assets/65953035-3c52-49c5-b474-fa5eb488b06f" />

- Redis sharding -  we'll shard based on user's score. We'll maintain the mapping between user_id and shard in application code. We can do that either via MySQL or another cache for the mapping itself.

- NO SQL 

<img width="1344" height="286" alt="image" src="https://github.com/user-attachments/assets/a689a9ef-9f33-468e-af41-c9c1ca067a4d" />
- This works well, but doesn't scale well if we need to query anything by score. Hence, we can put the score as a sort key:

<img width="1464" height="614" alt="image" src="https://github.com/user-attachments/assets/8df61645-9203-40dd-bf40-d1d38a1d06bb" />
- Another problem with this design is that we're partitioning by month. This leads to a hotspot partition as the latest month will be unevenly accessed compared to the others.
- We could use a technique called write sharding, where we append a partition number for each key, calculated via user_id % num_partitions
- An important trade-off to consider is how many partitions we should use:

The more partitions there are, the higher the write scalability
However, read scalability suffers as we need to query more partitions to collect aggregate results
- This NoSQL approach still has one major downside - it is hard to calculate the specific rank of a user.A cron job can periodically run to analyze score distributions, based on which a user's percentile is determined.

---
22.  Payment system
---
10. Uber
11. Tinder
12. Spotify
13. Bookmyshow
14. Goibibo/Skyscanner
15. Flipkart/E-commerce
<img width="1455" alt="image" src="https://github.com/user-attachments/assets/308213c6-8917-4276-9db1-a831dc69915b" />

16. Leetcode
18. Zoom
19. Google Pay/UPI
23. Airbnb
25. Stock Exchange
![image](https://github.com/user-attachments/assets/05d5724a-3c70-424a-bdef-b00d5dfe8e87)


    
