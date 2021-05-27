System design appraoch

1. Scope (FR, and Non-FR)
2. Key components
3. Data Model
4. Operation Cost


Scope:
1. given a stream of content/ items  which are being streamed, find top 10 songs
2. fast, highly available
...

e.g. 1000 songs in DB, and a streaming service which streams song,
we know which songs are streamed, need a count of how many are how many times, per sec,


2. songId, streamCount - (mul)
3. A-3
4. B-4
5. C-9
6. A-7

Request-> load balancer-> trendingservice () -> streamingservice (send out events)
trendingservice -  accept events, and maintain count for top 10 ,
			and contenders for top 10,
DB design: id, songId, stream count			
			
1. streamSong() 2. updateStreamCount() 3. getTop10Song()

Drawbacks of current system if viewed frm NonFR 
1. Fault tolerence (single DB, could break)
2. Availability (can accept only some request)
3. Scalability (not highly available), 
4. Performance (low, less request acceptance)
5. Logging and monitoring
6. Security (good, can be secured, indivoidually)
7. operation cost (min for now)
8. Accuracy (or consistency) high since single source of truth


Next STEP:
1. DB sharding, replicating data,
	1. SongTable - id, songId, urlLink    - based on songId can be shareded
	2. SongStreamTable - id, songId, streamCount 
	3. getTop10Songs() -> ask each shard to give 10 tops and sort and send back as request 
Have a redisCache updated with top 10 songs, by frequent query. 
Downside: not real time, even though scalable and highly available, fault tolerant, 



Next Step:
1. use redis cache for client request and update song v/s count count, send songs to a maxheap implementation, very fast

2. Use of apache spark:
3. Adding all services to auto scaling group allows scaling of services if CPU load increases a certai threshold like 80%.
4. Each song sends an event to apache spark to as stream of events 

getSong()-> createViewEvent()->Apache kafka-> store as well as forward to apache engine -> forward to redis