# Design Facebook Status Search

Facebook provides a search bar at the top of its page to enable its users to search posts, statuses, videos, and other forms of content posted by their friends and the pages they follow. In this question,

1.  Develop a service to enable the users to search the statuses posted on Facebook by their friends and followed pages.
2.  Consider that these statuses will only contain text for this particular question.


DESIGN GOALS:
Function requirments:
1. Service accepts a keyword, or a bunch of keywords and on action, click lists all the related search results which includes
	1. post, status, (forms of text) of friends and pages they follow
2.  Most recent on top, with relevant results.
Non-functional requirements:
1. Ensure minimum latency,
2.  High availability
3.  to ensure CAP properyt, for high availability and high partition tolerance, we can have eventual consistency
4.  Read-heavy operation, hence store in highly available and high read throughput DB

Scale Estimations: (just for reference as of now)
1. how many active users daily (searchers)? 1Billion,
2. how many searches per second? 30% are searching, 70 million searches/per second
3. We also need to index, new feeds, posts and pages infor updates
	1. each user has 200 friends and follow 100 pages,
	2. each user posts 2 post/avg (text) and pages post 10 times daily.


Approach:
Need to pre-process the data and create an index (inverted index of sort) to search for friends posts and pag
1. Let say Tony has 200 friends who posts 2 posts daily and he follows 100 pages which update page 10 times a day.
2. He searches for a keyword: Apple
	1. Get info from friends
		1. get id of all friends
		2. go through all the posts having the keyword (inverted index search)
		3. create a list and return 
	2. Get info from pages
		1. get id of all pages followed
		2. go through the content, feeds, posts having keyword(s) and match/filter  (inverted index search)
		3. create a list and return
	3. cut off on list of top 10/30 searches (based on device used)
	4. Use pagination, so if user reaches end of the first search, return next list


REST API specification
1. GET Request: /status/search
2. parameters:
	1. searchString
	2. userId
	3. search query (type as text for above case)
	4. timestamp (time of search)
	5. maxResults (based on device used )

Not part of (assuemed)
1. POST  /status/
2. content:
	1. path var: userId
	2. post content
	3. content type: encoded 
	4. timestamp



High Level Design
1. store all status (friends and pages) in DB
2. build an index for status, based on keyword - word v/s statusId, pagesId, userId