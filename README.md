# Week 10

## Graphs

Vertex ---> EDGE ---> Vertex  
**^ the above is ALWAYS directional**

actors -> ACT IN -> movie <- ACT IN <- actors

#### 7 Degrees of Kevin Bacon

This is what you would run in Neo4J:
```java
MATCH (p:Person {name: 'Kevin Bacon'})-
[*1..3]-(hollywood)
return DISTINCT p, hollywood
```
This is what you would run in SQL for the same above query:
```SQL
actors join moviesJobs join movies join moviesJobs
--or
actors in .. (actors in .. (actors in ..))
```

actor -> ACTS IN -> movie
ACTS IN {aID, mID}
the relation is many to many

**Breadth first search**
>So go from one node to another and visit all of the info in that node and keep adding that info to a queue therefore basically saying i have visited all of this info and nots there and keep moving to the next sorta thing

**Path**
>the weight or the value of path basically those edges

**Time**
>there can be a time to as to when this node visited this other node

**Tagging**
>so basically attaching a tag or a value to a thing in order to say this is something we want. Like say in a depth first visit a node may have the tag _visited_

**Set**
>you can also set a bunch of properties on nodes. Its an update query


## Neo4J Lingo

### Creating stuff

```java
//Neo4J example
CREATE (className:CLASS {stringName: 'theName', intBorn: 1900})

//java example
class className{
  String name = "theName";
  int born = 1900;
}
```
Another thing to emphasize is that in Neo4j we're prolly gonna be making classes that are singular that is those classes cant be reused again and again for other objects. For instance
```java
class Car{}
```
This class can have multiple different objects: Ferrari, Toyota, and so on

However,
```java
class KeanuReeves{}
```

Yeah thats just one guy so thats it we cant have two Keanu Reeves. I wish we could. Im sorry

### Creating Relations

```java
//Neo4J example
CREATE (KeanuReeves)-[:ACTED_IN {roles:'Neo'}]->(TheMatrix)
```
And my best java explanation is:
* KeanuReeves is a Class
* ACTED_IN is a Class or say a table in SQL
* ACTED_IN has the variables or properties,
  * String roles = "";
* TheMatrix is another Class

So since we are building a relationship it needds to go like this:
> object1 - [relationsName {relationsProperties}] -> object2  
> so object1 is related to object2 by thse properties

So the relationship is called an **Edge** and the name of the relation is the **Edge Label**  
_~~variable names~~ : labels_


### Doing Queries

**MATCH**
> This is like the select in SQL

So even our MATCH statements can be assigned a label

An example that we will hopefully get in Time
```java
MATCH(variableName:Label) return variableName
//can comment
match(m:movie) return m
```
Ok so we got it the above is the equivalent of
```java
for(m:movie){
  System.out.println(movie[m]);
}
```
OR
```SQL
SELECT * FROM movie
```

And then we also have
```java
MATCH (n) RETURN n
```

Which basically means return everything ever created in this table with all the relations described if any

So like in SQL it would had been
```SQL
SELECT * FROM table1
SELECT * FROM table2
SELECT * FROM table3
SELECT * FROM ...
SELECT * FROM tableN
```


So now we dont want all the ppl but a certain someone: "Tom Hanks"
```java
MATCH(p:Person{name:"Tom Hanks"})return p
```

This would read out as match in the tabel all p where p's name is "Tom Hanks"  
Therefore we made a variable which has the value Tom Hanks and we looked for them in Person and returned them  
So my MATCH statements label is _p_

_**Now this stuff is getting beautiful**_
```java
MATCH (v1:Node)-[r:SomeRelationship]-(v1:OtherNode) return v1,r,v2

//always have the ":" before the relation name
//so far its used every time
//and you can keep the last node/object/class bracket empty in order to indicate all a in relation b with everything
MATCH (a)-[:b]-(c) return a,b,c

MATCH (p:person)-[d:DIRECTED]-(m:Movie) RETURN p,d,m
```
So just looking at thelast one, I can say:  
Get me all persons (p) that have directed (d) a movie (m)


**SIZE**
> in Neo4J gives us the number of edges. (number of all outgoing edges)

```java
SIZE((a)-[r]-(b)) AS nameTheSizeVariable
ORDER BY nameTheSizeVariable
LIMIT 5
```

All in all:
```java
MATCH(a:Person)
Return a.name,
//here we add the above
SIZE((a)-[:r]-(b)) AS nameTheSizeVariable
ORDER BY nameTheSizeVariable DESC
LIMIT 5
```


Ok so now to make things a tad more specific
```java
MATCH(person1:Person{name: 'Tom Hanks'}), (person2:Person{name: 'Keanu Reeves'}),
p = shortestPath((person1)-[*..15]-(person2))
return p
```
* [*15] = exactly 15
* [*..15] = upto 15
* [*15..] = 15 and above
* [*1..15] = 1 to 15
* [*] = all of em

And we  can use **WHERE**
```java
MATCH (p:Person)-[d:DIRECTED]-(m:Movie)
  where m.released >2010
  RETURN p,d,m
```
