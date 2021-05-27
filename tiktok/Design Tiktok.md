Design a high level diagram of tiktok service.
A tiktok is a mobile application where users can share short videos. They can watch content on their feed. The feed can be mix of trending videos or from people that the user follows.
Short videos have some title, and description, also tags.

Out of scope currently: 1. Like, share, comment, search (by tag, by people)


Functional Requirement:
1.  User can CRUD a profile.
2. User can login, authentication, also logout.
3. User can upload a short video (30s to 1 min), with title and description.
4. User can view feed where they see videos from who they follow and trending.

Non-functional Requirement:
1. Availability - need high availability as social media app, 99.999%. 
2. Latency - can do, not much as well.
3. Fault tolerance  - high, once video upload, should not loose it, or trending videos views are not interuppted.
4. Ready heavy.

Calculations: Back-of-the envelope
Understand- bandwidth consumed, and rough estimate of  storage
Assumptions:-
1. 1 million users: 0.5 MB/user. 500GB.
2. 2 videos/day. 10Mb/day.  can use cloud provider storage service. 50k videos everyday. 18 million records/yr.


Rest API:
	User profile:
1. POST /user request: name, email, .. , response: return id.
2. GET /user/{id} profile details. 
3. PUT /user/{id} request: fields to update.
4. JWT token, or use any 3rd party auth tool.
5. PUT /user/{id}/follow/{id}

	Video Upload:
1. POST /user/{id}/upload, req: video metadata. return id
2. DELETE /user/{id}/video/{id}

	Feed generation:
	1. /feed
		1. list of video metadata, id, type: video streaming.


Database Design:
	1. User:
		userId (UUID)
		name (255)
		dateOfBirth (date)
		email
		phonenumber
		profile description
		profilePictureId
		
   2. User following:
	   userId along List:{ids of people/account you are  following, along with created, updated}
	index on userId,
	contraint on userId_followerId.
	
3. user video metadata
	  1. videoId
	  2. userId (index)
	  3. title
	  4. description
	  5. videoLink
	  6. videoStorageType
	  7. thumbailLink

 Feed Service:
	 1. userId, List<followerId>:  columarDB (any DB focuses on key value,)
	 2. trending database: (cache)
		1. videoId, userId.
	
	
HLD:
	 draw.io link: tiktok.drawio
	
user using a client application  (mobile app)

API Gateway to take care of DNS resolution, rate limiting, load balancer, and routing, ACL.
	
**profile service**: for user details, profile creation, and followers update.

**upload service**: 1. 720p -> low, medium, original resolution.
	video processing involved. divide into chunks and process it.
	upload it to S3 storage.
asynchronous: upload a video, then return videoId from user service.
	and upload service will return video url, and thumbnail link with saved metadata.
also do pre-processing of video.
	
based on intial resolution, covert the video to different resolution and handle compression.
	
**	Feed Service** : 

	will talk to read-only psql database
	we can add cache to pre-compute the few latest trending videos for users, so we reduce initiall latency for system.
	have a pre-process service to update cache. Could be on-demand or a batch process based on time when users are most active.

Cache: reduce load on DB. 
Why feed service and upload service different?
	We need a constant connection for upload service, write is much smaler read service in frequency terms.
	We need different scalability , as well as feed service is different domain.
	Since feed service, requires different algos for creating trending service.
	upload service, could hav some filter, restrictions and so on. Do update on video table.
	
	
**Follow up:**
what are the bottlenecks of the system for 10x traffic?

The read only database which stores user data and video data could take a hit along with the cache for feed service.
We could have multiple replicas of the read-only database, some kind of auto-scaling.
 Have a distributed caching service, and deploy feed service region specific to reduce network latency for geographic spread API calls. 
Regional data centers.
Also have distributed eventual consistent database. \
Make use of CDN, like cloudfront, or azure CDN.
Since videos are static, use CDN / S3 object store.

Database sharding based on userId.

Notes:
1. start with basic requirement
2. then address bottlenecks.