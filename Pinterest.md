A board with pictures, gif, and videos and create a board of all links and share it. 

Content shared is links, board can be shared to friends, have a private board.

Requirements:
 1. user can login to view various open pictures,
 2. user can login to view all current boards
 3. user can have friends, view and add friends
 4. user can create a board of certain topic and board pictures (inherently links)
 5. user can share the accessibilty of the board with friends.


Non-functional Requirements:
1. logging and monitoring
2. metrics: 
3. security
4. auth is taken care of:

Estimates:
Here we do not need to be accurte,
derive only: bandwidth, and storage 
1. 500 million  active montly user,
2. distributed across globe
3. active users per day: 1000, 
4. each user: 10 boards, total 1 million boards
5. friends: 100, total per user 110: boards. 
6. read heavy serive: 0.1 creaters, 0.9 readers

REST Endpoints: API
1. login service
	1. create account, login, forgot password, verify email
2. GET /board/view -> list of boards,
	1. images: name, id, quality attributes, src, link. {S3 storage}
3. GET  /board/{id} -> set of links, ordering of images (view definition)
4. GET /user/userId/friends
5. GET /user/profile/{userId} : friend profile.
6. POST /user/{userId}/board/{board-name}
	1. update: PUT /user/{userId}/board/link

7. POST /user/{userId}/board/{boardId} request: list of friends
8.  GET /user/{userId}/board/shared
	1.  


User Service, 	Board Service


Database Design:
1. user
	1. userId
	2. name
	3. email
	4. password hash
	5. reset password: flag

2. friends
	1. userId, friendId, created_timestamp, updated_timestamp, isActive true/false

3. Shared Board: 
	1. userId, boardId, updated, created, thumbnail, boardName

when you update	2. userId, boardId

Board service:
Tables:
- board: NoSQL DynamoDB. shard boardId, 
		1. boardId UUID
		2. ownerId UUID 
		3. name 255 VARCHAR
		4. description: 1k characters
		4. created + updated
	
- Link:
	1. id UUID, linkId URL, hash of image:
		metadata (json) -> title, etc, created_by
	
- BoardLink
	boardId, linkId - both UUID, have unique contraint on boardId_linkId
		also search index on boardId.
