
Core functionalities
1. post tweets (User can share text and images, 140 characters)
2. have followers
3. can follow other people (twitter account)
4. feed - updated with posts from people you follow, latest feed on top
5. profile management
6. Search Feed based on Hashtags

Non-functional Req:
1. scalable to 1million users
2. Highly available, eventual consistency, CAP.
3. DB req: 
	a. We do not need transactional capability, more BASE-ic than ACID.
	b. Since properties for user, tweet metadata could change, we need elastic structure for storing, than strict columns, 
4. More reads: 100 million users
		1. daily-> 5 feeds made by 1000 users
		2. avg: users per day-> 50 million users
		3. avg person has followers-> 1000
		4. avg. person follows-> 100 people
5.  needs caching for more recent, famous Tweets.



writes: 5 * 140 * 1000 -> 700 GB writes every day
read: 50 * 100 * 5*140 -> 3500GB reads every day
read:write ratio is 5:1.



API: 1. /tweet (create a tweet)
		2. /feed
		3. /{userId}/follow/{id}
		4. /{userId}/followers
		5. /{userId}/profile
		6. /search/tags/{#hashTag}
		
		
Persistance Layer:
1. User Profile Data: need C, so CP system (e.g. MongoDB, docTypeDB)
	1. Partition based on userId (since its random, could guarantee equal distribution)
	2. more frequent updates, and need to be consistent. 
2. Tweets- need high A, so AP system (e.g cassandra, columnarDB)
		1. text
		2. Image (externalize the storage) -> store links
		- needs to scale more / better than other data, also we can run background jobs on it, needs further processing, highly available, different DB system.
		- can sharding data based on userId as well, since processing generally would be by userId, and a user can have only limited tweets, is easy distribution.

Access pattern: more reads, and less updates. rread based system
3. users following mapping-> user v/s followers and vice versa
4. Indexing of tweets, based on tags
	1. we could store and manage in-house search but its better to re-use existing tech for string based search like ElasticSearch. see how it works.

