Given a stream of event v/s count, get count of each event, 
1. infinite stream
2. needs sublinear space and sublinear time (not grow as much as linear)

First Though:
1. use a hashMap to store
	1. solves problem but not always O(1) due to collision (2 event with same hash)
2. sampling
	1. can be made O(1), take every 5th value and compute, but loss of data and not accurate

TradeOff:
1. multiple hash functions, 
	e.g. for each event -> 4 hash, and increment value,
2. find count of event	
	1. get 4 hash of event-> get value, get min of set of values..

Min Count  - hence  and a matrix as below
	 0. 1 2 3 4 5 6
H1
H2
H3
H4
	