How to design database
Assumption and FR
1. In-memory DB
2.  login? (username and password) not main-
3.  create database
4.  create schemas within DBs
5.  create/delete/modify tables
6.  have columns with vartype and validation
7.  indexing? later
8.  query a particular table
9.  joins? later
10.  indexing? - later
11.  sequences? - later, add a counter for it
12.  query parser and validation
13.  query Interface
14.  authorizer
15.  give out result set,
16.  Stats?

NFR:
1. should be consistent
2. enable transactional?-later
3. table size? 100_000 records



Actors: 
1.admin (all permis) 
2. user with some permission

HLD:
Objects:
database
	schema
	table
		columns with constraints
		column<type, attribute> do validation
		persist
		indexing
Stats
		
		
Database: singleton, having multiple schemas
schema: List<Schema>,id, List<Table>
Table
		id	
		name
		Map<columnName, Contraint>
		List<Row>
		PrimaryKey
		UniqueConstraint
			-id
			-column
Row
	rowId
	columnValuesMap<Column, value>
	createdAt
	updatedAt
Column
	type
	name
	contraint
