## What is a Graph Database?

The formal definition is, a database that uses graph structures for semantic queries with nodes, edges, and properties to represent and store data.

## What are the benefits of using a Graph Database?

In graph databases the relationships between data are given as much (if not more) importance than the data itself. For specific types of data, a graph database can improve efficiency and perfomance significantly. Graph databases are extremely good at handling connected data - social media is a great example of this.

## What is Neo4j?

Neo4j is an open source graph database. Instead of using SQL, Neo4j uses a query language called [cypher](https://neo4j.com/developer/cypher/). If you are interested in this, take CS 476, where Neo4j is talked about more in depth. The source material for this section of the workshop comes largely from CS 476. 

### Lets go through an example with movies

The relationships between the data can be shown with the graph below

![image](https://user-images.githubusercontent.com/42680176/138567010-e65fc124-4263-47df-bd81-0be8c9e45360.png)

A cypher query example would be:

```
(:Person {name: string})-[:ACTED_IN {roles: [string]}]->(:Movie {title: string, released: number})
```

A visualization of the databse could be something like this:
![image](https://user-images.githubusercontent.com/42680176/138566977-7b2f4551-fe75-49f9-8ffe-0ad20db77cce.png)

Here are some examples of the Cypher query language:

In order to Find the labeled Person nodes in the graph
```
MATCH (p:Person)
RETURN p
LIMIT 1
```

In order to Find Person nodes in the graph that have a name of 'Tom Hanks'

```
MATCH (tom:Person {name: 'Tom Hanks'})
RETURN tom
```

This Cypher statement finds a movie by title and then returns for all people their name, possible roles, and the job (acted, directed, produced) as the first part of the lowercase rel-type ACTED_IN → acted.
```
MATCH (movie:Movie {title:$title})
OPTIONAL MATCH (movie)<-[rel]-(person:Person)
RETURN movie.title as title,
collect({name:person.name, role:rel.roles, job:head(split(toLower(type(rel)),'_'))}) as cast
LIMIT 1
```
for more info: https://neo4j.com/developer/get-started/
