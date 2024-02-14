# **Hands-on: neo4j**

> WORCK Training School 2 -- Bielefeld, February 15th, 2024

### What you will learn? 

How to install & configure neo4j Desktop; manage databases with neo4j Desktop & neo4j Browser; create, import, refactor & save data; query your graph & save query results.

### What you will not learn?

How to interact with online neo4j DBMS/via terminal; data modeling & queries optimization.

<details>
<summary>
Part 1: neo4j, graph theory and graph models
</summary>

### neo4j & graph theory

Neo4j is a graph database management system that can either be deployed online (community server edition/AuraDB) or locally (neo4j Desktop); it uses a language called `Cypher`; and it is based on graph theory.

<picture>
<img alt="YOUR-ALT-TEXT" src="https://graphacademy.neo4j.com/courses/neo4j-fundamentals/1-graph-thinking/1-seven-bridges/images/7-bridges.jpg"
  width="50%"
  height= auto>
</picture>

Leonhard Euler, 1736, Königsberg, East. Prussia asked a question: "Can we walk through the city and cross each of the seven bridges only once?". The answer was we cannot, but in the process, he defined the problem in abstract terms: a representation of land masses as entities (vertices) connected by bridges (edges).

### Two types of graph model

The Resource Description Framework (**RDF**) and the Label Property Graph (**LPG**) are two data models used to store data in the form of graphs. Both were developed in the 2000s and are more 'relational' & agile than so-called 'relational databases'.

Use cases examples: 

* route finding
* supply-chain analytics
* realtime recommendation
* network analysis (as done with Gephi)

The base of graph models are `semantic triples` (vertices connected by edges): `Subject—Predicate—Object`

In `Cypher` representation, a triple looks like this: `(:Subject)-[:Predicate]->(:Object)`

### RDF vs LPG pros and cons

| **The Resource Description Framework** | The Label Property Graph |
| :--- | :--- |
| stable storage and & **data exchange** | **storage & querying** |
| standardization (W3C specs), interoperability, scalability | less demanding (learning curve, development, and infrastructure) |
| ontologies & open vocabularies | entities have an internal structure: `labels`, `properties`, `types` |
| 'atomizes' information | **schema free** and **near natural language** |
| `vertices` and `edges` have no internal structure | flexible: allows progressive refactoring as the model evolves |
| indexed; uses 'artificial' vertices as joins | easy to 'read' and simpler |
| heavy infrastructure (sparql endpoints) | local deployment and availability of `apps` |

⚠️ no schema does not mean you do not have to think about your model and flexibility can quickly lead to messiness.

### LPG basics

The LPG model is made of **triples** composed of `nodes` linked by directed `relation(s)`:

* **nodes** have `Labels` & `Properties` (key-value pairs)
* **relations** are directed & have `Types` & `Properties`
* **queries** use `Cypher` instructions to traverse the graph, `MATCH` `patterns`, & `RETURN` results.

<picture>
<img alt="YOUR-ALT-TEXT" src="https://graphacademy.neo4j.com/courses/neo4j-fundamentals/2-property-graphs/1-property-graph/images/node-labels.jpg"
  width="50%"
  height= auto>
</picture>
</details>

<details>
<summary>
Part 3: First steps with neo4j
</summary>

### Installing neo4j Desktop

1️⃣ [https://neo4j.com/download/](https://neo4j.com/download/)  <br />
2️⃣ click on `download` <br />
3️⃣ fill in the form <br />
4️⃣ copy activation key <br />
5️⃣ install the downloaded file and enter the activation key <br />

### What does neo4j Desktop do?

Neo4j is only a possible "brick" in your digital workflow. The brick that stores your data in a structured form, but not your files (scans, transcripts, etc.). It allows you to:

* interact with a local (or online) DBMS using `Cypher`
* manage your graph databases: Create DBMS | Delete | Dump | Clone
* manage neo4j Desktop `Apps` & plugins
* manage files
* get help

If you want to learn more, there is plenty of online resources available:

* [the neo4j documentation](https://neo4j.com/docs/)
* [the neo4j developer tutorials](https://neo4j.com/developer/)
* [the Graph Academy](https://graphacademy.neo4j.com/>)
* [cheatsheets](https://mpolinowski.github.io/docs/Development/Graphs/2020-05-03--neo4j-cheat-sheet/2020-05-03/)
* neo4j Desktop integrated tutorials like `:play cypher` or `:play got`

### The LPG design process

1. understand your domain & define a use case scenario
2. develop an initial graph data model (nodes & relations)
3. test use cases against the initial data model
4. create the graph (instance model) & test data using Cypher
5. refactor in case of change or for better performance

**General tips**:

* avoid semantically orthogonal labels, (same `labels` for different types of nodes)
* avoid using `labels` that represent different levels of hierarchy:
  * e.g. Human & Person: hierarchies can be graphed (= overuse of `labels`)
  * `labels` & `types` are entry points for patterns (traversal efficiency)
* avoid duplicates. If `properties` appear many times in a node/relation, consider creating Labels/Types
* create specialized relationships on top of more generic ones
* stick to a limited number of controlled relation types & node labels

### First step: create a local database

1️⃣ `run` neo4j Desktop
2️⃣ on the left you see 3 icons: `Projects` | `DBMS` | `Graph Apps`
3️⃣ click on `Projects` (1) ➡️ `New` (2) ➡️ `Create Project`
4️⃣ `select` your project (3)
5️⃣ on the right, click `Add` (4) ➡️ `local DBMS`
6️⃣ give the local DB a `name` ➡️ set a `password` ➡️ choose a neo4j `version` ➡️ `Create` (this will take a few minutes)

⚠️ older versions of neo4j use older iterations of `Cypher`

### Awesome Procedures on Cypher (APOC)

APOC is an add-on library. It provides additional procedures & functions

We will use one `procedure` to allow export of the database to `CSV`

1️⃣ click on your database (1) ➡️ to uncover the panel on the right
2️⃣ click on `Plugins` (2)
3️⃣ click on the 🔻 under APOC (3)
4️⃣ click on `Install` (4) to install APOC procedures *for the selected database*

### Tweak the `apoc.conf` File

We also need to tell neo4j that it is allowed to use `apoc` procedures:

1️⃣ click the `...` (1) ➡️ `Open folder` (2) ➡️ `DBMS` (3)
2️⃣ the dbms folder opens -> then navigate to `conf` folder
3️⃣ in the `conf` folder, create a new `apoc.conf` file with a text editor
4️⃣ `paste` the following 3 lines and save the `apoc.conf` file

```txt
apoc.import.file.enabled=true
apoc.import.file.use_neo4j_config=true
apoc.export.file.enabled=true
```

### `START` the database**

⚠️ A DBMS must first be `STARTED`

1️⃣ click on the `...` next to its name <br />
2️⃣ click on `Start` <br />
3️⃣ wait...

⚠️ When the DBMS is started, DO NOT CLOSE THE TERMINAL!

### Open neo4j Browser

Once started, we can interact with a DBMS either via the `terminal` or using an `App`

We will use the `neo4j Desktop` app to manipulate the data with `Cypher`

1️⃣ on the left, click on the icon with 4 squares (`Gaph Apps`)
2️⃣ click on `neo4j Browser`

We are now ready to feed and query our database
</details>

<details>
<summary>
Part 3: Cypher basics
</summary>

### Representation

`Cypher` is a simple, descriptive & near natural language that uses ASCII representation:

* `nodes` are represented by `()`
* `relations` are represented by `-[]->`
* `patterns` are represented as follows: `()-[]->()`
* node `labels` & relation `types` are introduced with `:`
* nodes & relations `properties` are enclosed in `curly brackets {}`

### Conventions

- node `labels` are capitalized & preceded by a `:`
- relation `types` are written in `SNAKE_CASE_CAPITAL` & preceded by `:`
- `properties` are written in `camelCase`
- multiple properties (arrays) are separated by `,`
- `ids` are automatically attributed to entities (not stable)
- the first property associated with a node is its `primary key`
- `null` values are simply an absence of property (no need to specify null values)

### Examples

* `(:Object)` represents a node with an `object label`
* `(:Movie {title: 'The Matrix'})` represents a node with a `movie label` & a `title property` (a text string between quotes)
* `(matrix:Movie {title: 'The Matrix', released: 1997})` = same node with a second `property` (an integer) and a declared `variable`
* `-[role:ACTED_IN {roles: 'Neo'}]->` represents a `directed` relation with a `role variable`, a `roles` property & an `ACTED_IN type`

### Cypher clauses

`Cypher` relies on a limited number of explicit instructions and clauses:

* `CREATE` and `MERGE`
* `MATCH` and `RETURN`
* `REMOVE` and `DELETE`
* `SET`
* the `WHERE`, `WITH` or `CASE` clause

*we will only see a few
note that they can be chained and combined with `boolean` & `logical` operators*

### Variables

Like other languages, `Cypher` uses `variables` (or aliases) chosen arbitrarily by the user to collect & manipulate nodes, relations and properties

The `MATCH` instruction can be used to collect nodes with a particular label & `RETURN` the content of those nodes

In the following, we use a `c` variable to collect all nodes with an `Object` label. We return the selected nodes in a graph form with the `RETURN` instruction:

```cypher
MATCH (c:Object) 
RETURN c
```

### Types of properties

data types can be declared for `properties` upon creation or update (e.g., `toInteger(), toBoolean(), toFloat()` etc.)

- {String} text (between single or double quotes)
- {Long} integer value
- {Double} decimal value
- {Boolean}
- {Date} noted date('YYYY-MM-DD')
- {StringArray} comma-separated list of strings
- {DoubleArray} comma-separated list of decimal values
</details>

<details>
  <summary>
    Part 4: Create data
  </summary>

### The neo4j Desktop window

* on the left, the star allows you to `bookmark` code that works
* on top is the command line where you type `Cypher` code
* below are your previous instructions in card form
* on the left of each card are icons to `switch` between graph, text, or table `view`
* on the right of each card is an overview of the node labels and relation types
* the icons on the right allow you to:
  * bookmark or rerun the code
  * view in full screen
  * download your results in `JSON`, `CSV`, `PNG` and `SVG` formats

### Let's create our first node

We use the `CREATE` instruction to create nodes and relations.

We want our first node to be categorized as an 'Object' (we give it an `:Object` `label`); and to have three `properties`: `id`, `type`, & `name`. So, in the command line, type:

```cy
CREATE (:Object {id: 'obj0001', type: 'painting', name: 'Mona Lisa'})
```

### View our first node

To view our node, we `MATCH` it with an alias & `RETURN` it:

```cy
MATCH (c:Object)
RETURN c 
// What's happening here?
```

⚠️ To move from the first line to the next: `ENTER + SHIFT`

We could also `MATCH` ANY node to view the whole graph:

```cy
MATCH (n)
RETURN n
```

### Let's click on the `node` we created...

⚠️ In case you make mistakes, **there is no going back**, but if you can identify your error, it can usually be corrected

### Add some properties

Let us make Mona Lisa also a `Person` (another `label`) & give it a 'female' `property`

```cy
CREATE (:Person {id: 'obj0001', type: 'painting', name: 'Mona Lisa', sex: 'female'})
```

Let us look at the graph:

```cy
MATCH (n)
RETURN n
```

😓 Oops... We have 2 nodes with the same name: 1 with a `Person` label, 1 with an `Object` label.

### Let's delete our second node

1️⃣ we `MATCH` our target node <br />
2️⃣ we `DELETE` it:

```cy
MATCH (p:Person)
DELETE p
```

We can also use a `WHERE` clause:

```cy
MATCH (p)
WHERE p.sex = 'female'
DELETE p
```

### How to update node properties & labels?

1️⃣ we `MATCH` our target node (here, using a `WHERE` clause)
2️⃣ we `SET` properties and labels (we `chain` the 2 `SET` insctructions):

```cy
MATCH (p)
WHERE p.id = 'obj0001'
SET p.sex = 'male', p:Person
RETURN p
```

⚠️ Note the `,` in the `SET` clause (used to chain instructions)

### Let's correct our mistake

Mona Lisa should be 'female'; but perhaps she declared herself non-binary too:

```cy
MATCH (p)
WHERE p:Person
SET p.sex = 'female', p.gender = 'non-binary'
RETURN p
```

⚠️ `SET` either creates a new property or updates an existing one
⚠️ a `property` can be discarded by being `SET` to `NULL`

### Remove a property

To delete a property, we can either `SET` it to `null`:

```cy
MATCH (p:Person)
SET p.gender = null
RETURN p.name, p.gender
// What's happening here?
```

Or `REMOVE` it:

```cy
MATCH (p:Person {id: 'obj0001'})
REMOVE p.gender
RETURN p
// What's happening here?
```

⚠️ `DELETE` is only used to delete nodes and/or relations

### The `RETURN` clause

If you only specify the alias of a node in the `RETURN` clause, neo4j shows the corresponding nodes in graph form

We can also return properties of the aliased nodes as follows:

```cy
RETURN variable.property1 AS whateverYouWantToCallTheColumn, variable.property2
```

Example:

```cy
MATCH (p:Person)
RETURN p.name AS Alias, p.sex AS personGender
```

The result is a table that can be downloaded 

### A second node

Let us add `Leonardo` to the graph:

```cy
MERGE (p:Person {id: 'per0001', name: 'Leonardo da Vinci', sex: 'male'})
RETURN p
```

⚠️ `RETURN` is not necessary upon creation

⚠️ `MERGE` is another way to create entities 

💡 `MERGE` creates only IF entities with the exact same properties do not already exist
(it avoids duplication)

### Create a relation between two nodes**

We have created the nodes and now we want to relate them:

1️⃣ first, we `MATCH` the entities we want to manipulate
2️⃣ then, we `CREATE` the relation:

```cy
MATCH (leo:Person {id: 'per0001'})
MATCH (mona:Object {id: 'obj0001'})
CREATE (mona)-[:HAS_CREATOR {profession: 'painter'}]->(leo)
```

Let's look at the result:

```cy
MATCH (n)
RETURN n
```

### Delete relations and nodes

Like nodes, relations can be removed with `DELETE`

We `MATCH` a pattern, give an alias to the relation, and `DELETE` it:

```cy
MATCH (leo:Person {id: 'per0001'})-[r]-(mona:Object {id: 'obj0001'})
DELETE r
```

⚠️ In `MATCH` clauses, relations can be undirected

⚠️ Related nodes CANNOT be deleted before they are `DETACHED` from one another.

Let's clean our graph (delete everything):

```cy
MATCH (n)
DETACH DELETE n
```

### Create patterns

All the above process could have been done with one line of code:

```cypher
CREATE (:Person:Object {id: 'obj0001', type: 'painting', name: 'Mona Lisa', 
sex: 'female',gender: 'non-binary'})-[HAS_CREATOR {profession: 'painter'}]->
(:Person {id: 'per0001', name: 'Leonardo da Vinci', sex: 'male'}) 
```

oops... something is going wrong

Look for the 🔺 in the error messages can help (we forgot `:` in the relation)

```cypher
CREATE (:Person:Object {id: 'obj0001', type: 'painting', name: 'Mona Lisa', 
sex: 'female',gender: 'non-binary'})-[:HAS_CREATOR {profession: 'painter'}]->
(:Person {id: 'per0001', name: 'Leonardo da Vinci', sex: 'male'}) 
```

### A simple query

Querying the graph to visualize it (or parts of it) & to answer questions

Example of question: 

>What is the `sex` of `Persons` who declare themselves `non-binary` and is there a relation with the way they are labelled?

```cypher
MATCH (p:Person)
WHERE p. gender = 'non-binary'
RETURN p.sex AS sex, labels(p) AS labels
```

💡 Note the `labels(alias)` instruction in the `RETURN` clause

💡`id(variable)` would similarly return the `ids` created by the system

### Stop, export, dump and save your files

⚠️ Once you are finished, `STOP` the running DBMS before closing neo4j

⚠️ Your database lives in the neo4j folders, ➡️ better save your work:

* `dump` it (easy to reimport)
* or save it as a `csv` with `apoc` (import will be more burdensome)

## **Dump**

A `dump` file can be saved somewhere safe to be imported later:

1️⃣ close the neo4j Browser window <br />
2️⃣ `STOP` the database <br />
3️⃣ click on the `...` ➡️ `dump` (a dump file is added to your `files` list) <br />
4️⃣ click on the `...` ➡️ `Reveal in file explorer` <br />
➡️ copy the files somewhere safe

⚠️ importing `dump` files is quite easy ( you may have noted that the `...` also give you the option to create a DBMS from dump or import it in a DBMS)

### Export to `CSV`

Unlike `Dump`, export to `csv` is done in neo4j Browser (your DBMS has to be started)

If you have installed the `APOC` plugins & added the `apoc.conf` to your DBMS config:

```cy
CALL apoc.export.csv.all('filename.csv', {})
```

The exported `.csv.` is exported to the `import` folder of your DBMS...
</details>

<details>
<summary>
Part 5: Let us play with some more data
</summary>

If you want to do a littel more, a complete `Graph of Thrones` dataset can be directly played by running `:play got`

Let us `import` Characters of book 1 from 2 local `.csv` files:

* book1edgessample.csv
  * Headers: `Source`, `Target`, `Type`, `id` &  `weight`
* book1gendersample.csv
  * Headers: `Name`, `Nobility` & `Gender`

### Before we start...

Let's delete everything in our graph to make a fresh start:

```cy
MATCH (n)
DETACH DELETE n
```

⚠️ Remember: our two nodes are related, so we `DETACH` before we `DELETE`

Importing data from files means 1️⃣ `LOADING` a `.csv`, 2️⃣ `CREATING` nodes and relations, and 3️⃣ upon creation adding `properties`

### Uniqueness constraints

If needed, we can apply `uniqueness constraints`on properties that we want to be unique (like a unique identifier)

First, let us create a `UNIQUENESS CONSTRAINT` to make sure, one one property, that we have unique values (a unique id):

```cy
CREATE CONSTRAINT FOR (c:Character) REQUIRE c.name IS UNIQUE
// Query 01
```

To check existing straints in our graph: `SHOW CONSTRAINTS` or `:schema`

Drop a concstraint with its `name`: `DROP CONSTRAINT constraintName`



## **Load and check a `.csv` file**

1️⃣ place the `book1edgessample.csv` in the `import` folder
2️⃣ `LOAD` it and pass it to a `variable` to make it available for manipulation

Let's check if neo4j can read the file (using a `LIMIT` clause for efficiency)

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1edgessample.csv' AS line
RETURN line
LIMIT 10
// Query 02
```

<>

⚠️ instead of local files, we can also use an `http` address (ending by `.csv`)



## **Load and create the data from `.csv`**

`LOAD` does not import per se but makes the data available (using column `headers`)

To avoid creating duplicate nodes, we use `MERGE` instead of `CREATE`

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1edgessample.csv' AS row
MERGE (src:Character {name: row.Source})
MERGE (tgt:Character {name: row.Target})
MERGE (src)-[r:INTERACTS {weight: toInteger(row.weight)}]->(tgt)
// Query 03
```

The result should be `Added 100 labels, created 100 nodes, set 269 properties, created 169 relationships`

⚠️ note the `toInteger()` statement that defines weight data as integers



## **Add data from another `.csv` 1️⃣**

Let us view the graph:

```cy
MATCH (n)
RETURN n
```

Our nodes only have a `Character` label and a `name` property

How can we import `properties` from another file?

1️⃣ add the `book1gendersample.csv` file to the import folder
2️⃣ check whether it can be read

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1gendersample.csv' AS line
RETURN line
LIMIT 10
```



## **Add data from another `.csv` 2️⃣**

The file uses `Name`, `Nobility` and `Gender` as variables

We made sure that names in the `Name` column are the same as in our first dataset (they have to match)

We could try the following:

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1gendersample.csv' AS line
MERGE (c:Character {name: line.Name})
ON MATCH SET c.gender = line.Gender, c.house = row.Nobility
// Do not try this
```



## **Better instructions**

This would work but it would create duplicates of Character nodes

We do not need the `MERGE` instruction: we do not create new nodes or relations:

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1gendersample.csv' AS row
MATCH (c:Character {name: row.Name})
SET c.gender = row.Gender, c.house = row.Nobility
// Query 04
```

Let's check what this does:

```cy
MATCH (a:Character {name: 'Daenerys-Targaryen'}), (b:Character {name: 'Bran-Stark'})
RETURN a, b
```



## **View the data structure with APOC and view the graph**

View the graph model (with `apoc` procedure):

```cy
CALL apoc.meta.graph()
```

View the whole graph by matching a `pattern`:

```cy
MATCH p=(:Character)-[:INTERACTS]-(:Character)
RETURN p
```

OR simply view all:

```cy
MATCH (n)
RETURN n
```



## **Analyze the data**

We can now query the graph to analyze our data

We could also do simple stats (average and standard deviations): 

```cy
MATCH (c:Character)-[:INTERACTS]->()
WITH c, count(*) AS num
RETURN min(num) AS min, max(num) AS max, avg(num) AS avg_characters, stdev(num) AS stdev
// where count() counts the number of character nodes who have relations with other nodes
```

And much more, like centrality measures, shortest paths, pivotal nodes, etc.



## **Let's do some counting 1️⃣**

How many Character nodes in the database?

```cy
MATCH (c:Character) 
RETURN count(c)
```

What is the most common number of interactions?

```cy
MATCH ()-[r]->()
RETURN r.weight, count(*)
ORDER BY count(*) DESC
// Query 05
```



## **Let's do some counting 2️⃣**

How many Character nodes have a `male` property?

```cy
MATCH (n:Character {gender: 'Male'}) 
RETURN count(n) AS numberOfMaleCharacters
```

Let's use a `WITH` clause to count Male and Female Characters and their ratio:

```cy
MATCH (m:Character {gender: 'Male'})
WITH count(m) AS numMales
MATCH (f:Character {gender: 'Female'})
WITH count(f) AS numFemales, numMales
RETURN numMales, numFemales, toFloat(numFemales) / numMales AS genderRatio
// Query 06
```



## **Let's do some counting 3️⃣**

How many people does one character interact with?

```cy
MATCH (c1)-[r:INTERACTS]->(c2)
RETURN c1.name as name,
COUNT(*) AS Total
```

Can we count the number of interactions of one Character and get a list of the people they interact with? (`COLLECT`)

```cy
MATCH (c1)-[r:INTERACTS]->(c2)
RETURN c1.name as Person,
COUNT(*) as Total,
COLLECT(c2.name)
// Query 07
```



## **Let's do some exploration**

![bg left 70%](explore1.png)

Let us identify 
1️⃣ who interacts with Bran Stark besides Arya Stark 
2️⃣ who interacts with both of them



## **Identify the right pattern to traverse**

...to do so, we need to identify the pattern that Cypher must `traverse` to return only the results we want:

```cy
MATCH (a:Character {name: 'Arya-Stark'})-[r1]-(b:Character {name: 'Bran-Stark'})-[r2]-(c:Character)
RETURN c.name
// Query 08
```

And:

```cy
MATCH (a:Character {name: 'Arya-Stark'})-[r1]-(b)-[r2]-(c:Character {name: 'Bran-Stark'})
RETURN b.name
// Query 09
```



## **Ask questions**

Who in the database has the same `gender` property as Arya Stark?

```cy
MATCH (c:Character), (c1:Character {name: 'Arya-Stark'}) 
WHERE c.gender = c1.gender AND id(c) <> id(c1)
RETURN c.name
// Query 10
```

Are there nodes for which `gender` data is missing?

```cy
MATCH (c:Character)
WHERE c.gender IS NULL
RETURN c.name
```

Is the data consistent?

```cy
MATCH (n)
RETURN DISTINCT n.gender
//we can do the same with n.house to check house names
```



## **Let's practice**

Which Characters have a name that starts with 'J'?

```cy
MATCH (p:Character)
WHERE p.name STARTS WITH 'J'
RETURN p.name
```



## **`CASE` clauses**

We would like to export a dataset of the gender distribution by house to plot it. To do this, we need a less simple clause: `CASE` 

> A `CASE` clause introduces a condition (`WHEN`) and instructions:

```cy
MATCH (c:Character)
RETURN c.house AS House, 
COUNT(CASE WHEN c.gender='Male' THEN 1 END) AS NumberMales, 
COUNT(CASE WHEN c.gender='Female' THEN 1 END) AS NumberFemales, 
COUNT(CASE WHEN c.gender IS NULL THEN 1 END) AS NoGender;
// Query 11
```



## **Let's `ORDER` our results'**

Let us order our results alphabetically with `ORDER BY` (2 options):

```cy
MATCH (c:Character) 
RETURN c.house AS House, 
COUNT(CASE WHEN c.gender='Male' THEN 1 END) AS NumberMales, 
COUNT(CASE WHEN c.gender='Female' THEN 1 END) AS NumberFemales, 
COUNT(CASE WHEN c.gender IS NULL THEN 1 END) AS NoGender 
ORDER BY House DESC
```

```cy
MATCH (c:Character)
WITH c ORDER BY c.house
RETURN c.house AS House, 
COUNT(CASE WHEN c.gender='Male' THEN 1 END) AS NumberMales, 
COUNT(CASE WHEN c.gender='Female' THEN 1 END) AS NumberFemales, 
COUNT(CASE WHEN c.gender IS NULL THEN 1 END) AS NoGender
```



## **Add dates**

Let's add a few (random) dates to our dataset with `date('YYYY-MM-DD')

```cy
MATCH (a:Character {name: 'Arya-Stark'})
MATCH (b:Character {name: 'Jon-Snow'})
SET a.dob = date('0300-09-23'), b.dob = date('0290-06-12')
// Query 12
```

And add another date property to a Character (we just know the year)

```cy
MATCH (c:Character {name: 'Bronn'})
SET c.dob = date({year: 295}), c.dod = date({year: 320})
RETURN c
// Query 13
```

⚠️ click on the node and check how the date was interpreted...



## **Test the existence of properties**

Let's test the existence of female Characters with date of birth:

```cy
MATCH (p:Character)
WHERE p.gender = 'Female'
AND p.dob IS NOT NULL
RETURN p.name AS name, p.gender AS Gender, p.house AS Alliegence
// Query 14
```

Let's check relations that have a certain `weight` property:

```cy
MATCH (a)-[p]->(b)
WHERE p.weight IN [1, 5, 7]
RETURN a.name AS Name, p.weight AS Weight, b.name AS Target
// Query 15
```


## **Play with dates**

Bronn has a date of birth and a date of death. How old was he when he died?

```cy
MATCH (c:Character {name: 'Bronn'})
RETURN duration.between(c.dob,c.dod) // Query 16
```

The calculation is provided in `Cypher` notation.
But we can use `year(s)` (and `day(s)` or `month(s)`) arguments: 

```cy
MATCH (c:Character {name: 'Bronn'})
RETURN duration.between(c.dob,c.dod).years
```

Who was born between the years 200 and 400?

```cy
MATCH (c)
WHERE c.dob IS NOT NULL
AND c.dob.year >= 200 <= 400
RETURN c.name As Character, c.dob AS Born // Query 17
```



## **Refactor**

1️⃣ add a second value to a property

```cy
MATCH (c:Character {name: 'Jon-Snow'})
SET c.house = ['Stark', 'Nights-Watch']
RETURN c.name as Name, c.house as Houses
```

Look at the result and see how this created an array of values for the `house` property

2️⃣ change a property name in bulk. We want to change `gender` to `sex` but keep the values:

```cy
MATCH (c:Character)
SET c.sex = c.gender
REMOVE c.gender
RETURN c
```



## **Transform properties into nodes**

Case: Our characters have a gender but we want gender to be a specifi node instead of a property (we are thus changing the sctructure of our graph). We can do this in two steps (perhaps 1):

```cy
MATCH (n:Character)
WHERE n.gender IS NOT NULL
WITH n.gender AS gender
MERGE (g:Gender {value: gender})
RETURN g
// the IS NOT NULL clause is crucial: neo4j cannot create nodes from *null* values
```

```cy
MATCH (n:Character)
MATCH (m:Gender)
WHERE m.value = n.gender
CREATE (n)-[:HAS_GENDER]->(m)
REMOVE n.gender
```



## **Troubleshooting**

* There is a lot on the neo4j website (see earlier)

* About temporal values, see: [https://neo4j.com/docs/cypher-manual/current/values-and-types/temporal/](https://neo4j.com/docs/cypher-manual/current/values-and-types/temporal/)

* Internet is your friend

* Chat assistants are quite good at providing Cypher queries

