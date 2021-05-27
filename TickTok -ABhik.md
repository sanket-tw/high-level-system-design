Design a tiktok system


STEP-1 : Define what is in scope and what is out of scope, then you have a list of features you want to develop within given time limits:
1. User profile
2. login capability
3. Uploading short videos
4. having followers and follow
5. Viewing short videos
	1. page where all chronologically updated short videos come up .
	2. maybe paginate results

NOTE: also define scalabiity, might help containing app to geo so we don't have to expand on geo as well
out of scope:
1. feed, like 
2. 6. react to posts (short videos)
3. 6. search by profile name, video tags (return a paginated search)


STEP-2: define use cases, and actors

Actor: 1. user 2. admin (out of scope)

user : user profile
		1. create a profile,
			name, age, email-id, KISS pic
		2. update 3. delete
	NOTE: no profile PIC, that is orthogonal requirement, not time focuses, tiktok main part is video processing, so focus on that
		
login: 1. sign up, (username, firstname, lastname, email id) {maybe use email verify, but KISS we just provide password}
			2. log in (username, password, KISS so no forgot password/username?)
				- generate a JWT token / cookie / oAUTh (KISS so JWT)
		

upload SV: 1. upload/(could be internal) 2. validate/ check size, user details, auth, filter 
					to simple storage, like S3
					2. for all followers of user, we update the feed with video link
					- different quality and resolution for videos,
		     		- based on bandwidth we use APIs to upload videos
					- we can get the bandwidth from server and 
					
4. /follow/{id}/following/{id2}  follow user, and /follow GET get all followers,  GET /followers/{id}
5. /feed/{id}
	1. search through followers and update the videos


Estimates:
1. how many users? 100 million
2.  profile data per user? 300 bytes
3.  video size? 10sec ~ 50 MB for 100000 users
4.  user uploads 2 videos per day
5.  user has 100 followers, 
6.  follower views 10 videos daily

Profile DB: 10^9 * 150 = 14,000 GB data 
video: daily increase: 24GB everyday
follow streams: ~ 1GB at most for each follower

DB?
why NoSQL?
	1. SQL is fine, as we know fields and what to query, but we do not need transactional capability , need better / higher availability and scalable
	2. NoSQL: 
		a. predicatable read and writes, 
		b. no joins required,
		c. fetch and provide from DB., APIs do mostly same thing over and over again.
	NOTE: also read intensive mostly, not much updates.
	3. faster fetch by profile by sharding by key [SQL needs index, but NoSQL provide OOTB], location based sharding. (might be out of scope and not work with millions of followers)


upload video:
1. upload and convert
2. video converter to multiple formats
	1. load balancer in front, to divert request based on uploaded video
	2. object store


View videos:
1. use CDN for most followed user
	1. CDN manager? cloudflare or akamai,
	2. server decides the resolution and returns a link
NOTE: think about user experience, how to reduce latency, make highly available


Enhancement:
1. identify a duplicate video
	1. how by getting hash of video, like checksum or MD5 hash


Leanring:
1. limit scope
2. stick to basic of what we know
3. 