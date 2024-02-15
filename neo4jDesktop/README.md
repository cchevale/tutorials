# **Hands-on: neo4j**

> WORCK Training School 2 -- Bielefeld, February 15th, 2024

This session is not designed to push you to use a specific tool. Our digital workflows are not about the tools that we use but about the questions we want to answer. Graph modeling **might or might not** be something that you want to use as part of your workflow. 

### What you will learn? 

- How to install & configure neo4j Desktop
- Manage databases with neo4j Desktop & neo4j Browser
- Create, import, refactor & save data
- Query your graph & save query results

### What you will not learn?

- How to interact with an online neo4j DBMS or via a terminal
- Data modeling & queries optimization

<details>
<summary>
Part 1: Neo4j, graph theory and graph models
</summary>

### Neo4j & graph theory

Neo4j is a graph database management system that can either be deployed online (on a community edition server or an AuraDB) or locally (neo4j Desktop). It uses a language called `Cypher` and it is based on graph theory.

<picture>
<img alt="YOUR-ALT-TEXT" src="https://graphacademy.neo4j.com/courses/neo4j-fundamentals/1-graph-thinking/1-seven-bridges/images/7-bridges.jpg"
  width="50%"
  height= auto>
</picture>

For those who don't know, graph theory is attributed to Leonhard Euler, from Königsberg (East. Prussia), who asked the following question in 1736: "Can we walk through the city and cross each of the seven bridges only once?". The answer was 'no', but in the process, he defined the problem in abstract terms, that is as, a representation of land masses as entities (vertices) connected by bridges (edges).

### Two types of graph model

There are two main types of graph models around nowadays: the Resource Description Framework (**RDF**) and the Label Property Graph (**LPG**), which are used to store data in the form of graphs. Both were developed in the 2000s and can be said to be more "relational" & agile than so-called "relational databases" (like Filemaker). Graphs are everywhere nowadays. Use cases examples include: 

* route finding
* supply-chain analytics
* realtime recommendation
* network analysis (as done with Gephi)

The base of graph models are `semantic triples`: 2 vertices (one semantic "subject" and one semantic "object") connected by an edge: `Subject—Predicate—Object`.

In `Cypher` representation, a triple would look as follows: `(:Subject)-[:Predicate]->(:Object)`

### RDF vs LPG (pros and cons)

| **The Resource Description Framework** | The Label Property Graph |
| :--- | :--- |
| stable storage and & **data exchange** | **storage & querying** |
| standardization (W3C specs), interoperability, scalability | less demanding (learning curve, development, infrastructure) |
| ontologies & open vocabularies | entities have an internal structure: `labels`, `properties`, `types` |
| "atomizes" information according to a preset schema | **schema free** and **near natural language** |
| `vertices` and `edges` have no internal structure | flexible: allows progressive refactoring as the model evolves |
| indexed; uses 'artificial' vertices as joins | easy to 'read' and simpler to implement and use |
| heavy infrastructure (sparql endpoints) | local deployment and availability of `apps` |

⚠️ No schema does not mean that you do not have to think about your model; and flexibility can quickly lead to messiness.

### LPG basics

The LPG model is made of **triples** composed of `nodes` linked by directed `relation(s)`:

* **nodes** have `Labels` & `Properties` (key-value pairs, as in a python dictionary)
* **relations** are directed & have `Types` & `Properties`
* **queries** use `Cypher` instructions to traverse the graph, `MATCH` patterns, & `RETURN` results.

<picture>
<img alt="YOUR-ALT-TEXT" src="https://graphacademy.neo4j.com/courses/neo4j-fundamentals/2-property-graphs/1-property-graph/images/node-labels.jpg"
  width="50%"
  height= auto>
</picture>
</details>

<details>
<summary>
Part 2: Setting up neo4j
</summary>

### Installing neo4j Desktop

1️⃣ [https://neo4j.com/download/](https://neo4j.com/download/)  <br />
2️⃣ click on `download` <br />
3️⃣ fill in the form <br />
4️⃣ copy the activation key <br />
5️⃣ install the downloaded file and enter the activation key <br />

### What does neo4j Desktop do?

Neo4j is only a possible "brick" in your digital workflow. The brick that stores your data in a structured form, but not your files (scans, transcripts, etc.). Neo4j Desktop is a piece of software developed by the neo4j company, that allows you to:

* interact with a local (or online) DBMS using `Cypher`
* manage your graph databases (you can create DBMS, create and refactor entities, delete, cump and clone your DBMS)
* manage neo4j Desktop `Apps` & plugins
* manage files like `.csv` or `cypher` files
* get help

If you want to learn more, there is plenty of resources available online:

* [the neo4j documentation](https://neo4j.com/docs/)
* [the neo4j developer tutorials](https://neo4j.com/developer/)
* [the Graph Academy](https://graphacademy.neo4j.com/>)
* [cheatsheets](https://mpolinowski.github.io/docs/Development/Graphs/2020-05-03--neo4j-cheat-sheet/2020-05-03/)
* [another cheatsheet on GitHub](https://github.com/bitfede/cypher-cheat-sheet/blob/master/README.md)
* neo4j Desktop's integrated tutorials like `:play cypher` or `:play got`

### The LPG design process

As neo4j is schema-free, you do not need to have a predefined model to start feeding a DBMS with data. An LPG design process will in general involve the following steps:

1️⃣ understand your domain & define a use case scenario <br />
2️⃣ develop an initial graph data model (nodes & relations) <br />
3️⃣ test use cases against the initial data model <br />
4️⃣ create the graph (instance model) & test data using `Cypher` <br />
5️⃣ refactor in case of change or for better performance <br />

**General tips**:

* avoid semantically orthogonal labels, (same `labels` for different types of nodes)
* avoid using `labels` that represent different levels of hierarchy:
  * e.g. Human & Person: hierarchies can be graphed (= overuse of `labels`)
  * `labels` & `types` are entry points for patterns (traversal efficiency)
* avoid duplicates. If `properties` appear many times in a node/relation, consider creating Labels/Types
* create specialized relationships on top of more generic ones
* stick to a limited number of controlled relation types & node labels

### First step: create a local database

1️⃣ `run` neo4j Desktop <br />
2️⃣ on the left you see 3 icons: `Projects` | `DBMS` | `Graph Apps` <br />
3️⃣ click on `Projects` ➡️ `New` ➡️ `Create Project` <br />
4️⃣ `select` your project <br />
5️⃣ on the right, click `Add` ➡️ `local DBMS` <br />
6️⃣ give the local DBMS a `name` ➡️ set a `password` ➡️ choose a neo4j `version` ➡️ `Create` (this will take a few minutes) 

⚠️ `Cypher` is an evolutive language: older versions of neo4j use older iterations of `Cypher` (meaning that code snippets that you may find online can sometimes be deprecated)

### Awesome Procedures on Cypher (APOC)

In addition to neo4j's built-in functionalities, users can use the so-called Awesome Procedures on Cypher (a.k.a. APOC). APOC is an add-on library that provides additional procedures & functions. We will use one `procedure` to allow the export of a database to `CSV`. To do so, APOC plugins need to be installed on your DBMS. 

1️⃣ click on your database to uncover the panel on the right <br />
2️⃣ click on `Plugins` <br />
3️⃣ click on the 🔻 under APOC <br />
4️⃣ click on `Install` to install APOC procedures *for the selected database*

### Tweak the `apoc.conf` File

To be able to use APOC instructions, we also need to tell neo4j that it is allowed to use `apoc` procedures by creating and editing a configuration file called `apoc.conf`:

1️⃣ click the `...` ➡️ `Open folder` ➡️ `DBMS` <br />
2️⃣ the DBMS folder opens ➡️ navigate to the `conf` folder <br />
3️⃣ in the `conf` folder, create a new `apoc.conf` file with a text editor <br />
4️⃣ `paste` the following 3 lines in it and save

```txt
apoc.import.file.enabled=true
apoc.import.file.use_neo4j_config=true
apoc.export.file.enabled=true
```

### `START` the database

⚠️ Before you can use your DBMS, you must `START` it:

1️⃣ click on the `...` next to its name <br />
2️⃣ click on `Start` <br />
3️⃣ wait...

⚠️ Once a DBMS is started a Terminal instance opens. **DO NOT CLOSE THE TERMINAL!**

### Open neo4j Browser

Once the DBMS runs, we can interact with it either via the `terminal` or using an `App`. We will use the `neo4j Desktop` app to manipulate the data with `Cypher`:

1️⃣ on the left, click on the icon with 4 squares (`Gaph Apps`) <br />
2️⃣ click on `neo4j Browser`

We are now ready to feed and query our database!
</details>

<details>
<summary>
Part 3: Cypher basics
</summary>

### Representation

`Cypher` is a simple, descriptive & near natural language that uses ASCII representation (='.'=):

* `nodes` are represented by `()`
* `relations` are represented by `-[]->`
* `patterns` are represented as follows: `()-[]->()`
* node `labels` & relation `types` are introduced with `:`
* nodes & relations `properties` are enclosed in `curly brackets {}`

### Conventions

- node `labels` are `capitalized` & preceded by `:`
- relation `types` are written in `SNAKE_CASE_CAPITAL` & preceded by `:`
- `properties` are written in `camelCase`
- multiple properties (arrays) are separated by `,`
- `<ids>` are automatically attributed to entities (not stable)
- the first property associated with a node is its `primary key`
- `null` values are simply an absence of property (no need to specify null values)

### Examples

* `(:Object)`
  * represents a node with an `Object label`
* `(:Movie {title: 'The Matrix'})`
  * represents a node with a `Movie label` & a `title property` (with a string "The Matrix" as a value)
* `(matrix:Movie {title: 'The Matrix', released: 1997})`
  * same node with a second `property` (with an integer as a value) and a declared `variable` (*matrix*)
* `-[role:ACTED_IN {roles: 'Neo'}]->`
  * represents a `directed` relation with a `role variable`, an `ACTED_IN type` & a `roles` property

### Cypher clauses

`Cypher` relies on a limited number of explicit instructions and clauses (written with CAPITALS):

* `CREATE` and `MERGE` (`CREATE` will create new data whatever is in the graph)
* `MATCH` and `RETURN` (in a `MATCH` clause you define the parameters of how Cypher traverses the graph and `RETURN` defines what output you want to get)
* `REMOVE` and `DELETE` (`REMOVE` a property; `DELETE` an entity)
* `SET` (used to update properties of entities)
* the `WHERE`, `WITH` or `CASE` clause

*We will only see a few clauses. Note that they can be chained and combined with `boolean` & `logical` operators.*

### Variables

Like other languages (such as Python), `Cypher` uses `variables` (or aliases) chosen arbitrarily by the user to collect & manipulate nodes, relations and properties.

For instance, we can use a variable in a `MATCH` instruction to collect nodes with a particular label & `RETURN` the content of those nodes with the variable. Variables can be passed arguments (we will see that later). In the following, we use a variable that we name `c` to collect all nodes with an `Object` label. We return the selected nodes in a graph form with the `RETURN` instruction:

```cypher
MATCH (c:Object) 
RETURN c
```

### Types of properties

Data types can be declared for `properties` upon creation or update (e.g., `toInteger()`, `toBoolean()`, `toFloat()` etc.). Cypher can interpret different data types, including: `date`, `durations`, `dateTime`, `string`, `long`, `boolean`, `stringArray` etc.

</details>

<details>
  <summary>
    Part 4: Create data
  </summary>

### The neo4j Desktop window

The easiest way to interact with a neo4j DBMS is to do this in a neo4j Desktop window. Remember that you must `START` a DBMS before you can use it. Let's have a look at what the neo4j Desktop interface look like:

* on the left, the star allows you to `bookmark` code that works
* on top is the command line where you type and execute `Cypher` code
* below are your previous instructions in card form
* on the left of each card are icons to `switch` between graph, text, or table `view`
* on the right of each card is an overview of your node labels & relation types
* the icons on the right allow you to:
  * bookmark or rerun a `Cypher` code
  * view your graph in full screen
  * download your results in `JSON`, `CSV`, `PNG` & `SVG` formats

### Let's create our first node

We use the `CREATE` instruction to create nodes & relations.

Let us create our first node. We want it to be categorized as an 'Object' (i.e., give it an `:Object` `label`); & to have three `properties`: `id`, `type`, & `name`. Remember that node labels & relation types are introduced by `:`. Properties are always embedded in a node's `()` or a relation's `[]` & written as `key-value pairs` between `{}`. So, in the command line, type:

```cy
CREATE (:Object {id: 'obj0001', type: 'painting', name: 'Mona Lisa'})
```

The values of the properties of our node are of a `string` data type (in quotes). Now, to view our first node, we can use a set of parameters to `MATCH` it, pass it to an alias & `RETURN` it:

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

Let's click on the `node` we created: neo4j Desktop provides a few options to look at it in more detail.

⚠️ In case you make mistakes, **there is no going back**, but if you can identify your error, it can usually be corrected

### Add some properties

Nodes can have multiple labels. Let us make Mona Lisa also a `Person` (another `label`) & give it a 'female' `property`:

```cy
CREATE (:Person {id: 'obj0001', type: 'painting', name: 'Mona Lisa', sex: 'female'})
```

Let us look at the whole graph to see what it did:

```cy
MATCH (n)
RETURN n
```

😓 Oops... We have 2 nodes with the same name property: 1 with a `Person` label, 1 with an `Object` label. So, we might want to delete our second node. To do so, we `MATCH` our target node, and instead of returning it, we `DELETE` it:

```cy
MATCH (p:Person)
DELETE p
```

We can also use a `WHERE` clause:

```cy
MATCH (p)
WHERE p.sex = 'female'
DELETE p
// What's happening here?
```

### How to update node's properties & labels?

To update a node's property or its label(s):

1️⃣ we `MATCH` our target node (here, using a `WHERE` clause) <br />
2️⃣ we `SET` properties & labels

```cy
MATCH (p)
WHERE p.id = 'obj0001'
SET p.sex = 'male', p:Person
RETURN p
```

⚠️ Note the `,` in the `SET` clause, which is used to chain instructions.

### Let's correct our mistake

Mona Lisa should be 'female'; but perhaps she declared herself non-binary too:

```cy
MATCH (p)
WHERE p:Person
SET p.sex = 'female', p.gender = 'non-binary'
RETURN p
```

`SET` either creates a new property or updates an existing one. A `property` can be discarded by being `SET` to `null` value.

### Remove a property

To delete a property, we can `SET` it to `null`:

```cy
MATCH (p:Person)
SET p.gender = null
RETURN p.name, p.gender
// What's happening here?
```

We can also `REMOVE` it:

```cy
MATCH (p:Person {id: 'obj0001'})
REMOVE p.gender
RETURN p
// What's happening here?
```

`DELETE` is only used to delete nodes and/or relations.

### The RETURN clause

If you only specify the alias of a node (or nodes) in the `RETURN` clause, neo4j shows the corresponding node(s) in graph form. We can also return properties of the aliased nodes as follows:

```cy
RETURN variable.property1 AS whateverYouWantToCallTheColumn, variable.property2
```

Example:

```cy
MATCH (p:Person)
RETURN p.name AS Alias, p.sex AS personGender
```

The result is a table that can be downloaded.

### Add a second node

Let us add `Leonardo` to the graph:

```cy
MERGE (p:Person {id: 'per0001', name: 'Leonardo da Vinci', sex: 'male'})
RETURN p
```

`RETURN` is not necessary upon creation

`MERGE` is another way to create entities. It will create an entity ONLY IF entities with the exact same properties do not already exist (it avoids duplication).

### Create a relation between two nodes

We have created the nodes & now we want to relate them:

1️⃣ first, we `MATCH` the entities we want to manipulate <br />
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

### Delete relations & nodes

Like nodes, relations can be removed with the `DELETE` instruction: We `MATCH` a pattern, give an alias to the relation, & `DELETE` it:

```cy
MATCH (leo:Person {id: 'per0001'})-[r]-(mona:Object {id: 'obj0001'})
DELETE r
```

In `MATCH` clauses, relations can be undirected.

⚠️ Related nodes CANNOT be deleted before they are `DETACHED` from one another. Let's clean our graph (delete everything):

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

😓 Oops... something is going wrong. Look for the 🔺 in the error messages can help (we forgot `:` in the relation)

```cypher
CREATE (:Person:Object {id: 'obj0001', type: 'painting', name: 'Mona Lisa', 
sex: 'female',gender: 'non-binary'})-[:HAS_CREATOR {profession: 'painter'}]->
(:Person {id: 'per0001', name: 'Leonardo da Vinci', sex: 'male'}) 
```

### A simple query

Besides creating entities and updating properties, `Cypher` is also used to query the graph, to visualize it (or parts of it) & to answer questions.

How to answer the following question (provided that we have a sufficient amount of data for it to make sense): What is the `sex` of `Persons` who declare themselves `non-binary` & is there a relation with the way they are labelled?

```cypher
MATCH (p:Person)
WHERE p. gender = 'non-binary'
RETURN p.sex AS sex, labels(p) AS labels
```

💡 Note the `labels(alias)` instruction in the `RETURN` clause, `id(variable)` would similarly return the `ids` created by the system.

### Stop, export, dump & save your files

Once you are finished, `STOP` the running DBMS before closing neo4j. 

Your database lives in the neo4j folders. You might want to make copies of it and save your work. We can save everything in the graph with `dump` (easy to reimport), or as a `csv` with `apoc` (although import will be more burdensome, as we will see in the next part).

### Dump

A `dump` file can be saved somewhere safe to be imported later:

1️⃣ close the neo4j Browser window <br />
2️⃣ `STOP` the database <br />
3️⃣ click on the `...` ➡️ `dump` (a dump file is added to your `files` list) <br />
4️⃣ click on the `...` ➡️ `Reveal in file explorer` <br />
➡️ copy the files somewhere safe

Importing `dump` files is quite easy (you may have noted that the `...` also give you the option to create a DBMS from dump or import a dump file in a DBMS)

### Export to `CSV`

Unlike `Dump`, export to `csv` is done in neo4j Browser (your DBMS has to be started). If you have installed the `APOC` plugins & added the `apoc.conf` to your DBMS config, you can simply execute the following commands:

```cy
CALL apoc.export.csv.all('filename.csv', {})
```

The `.csv.` is exported to the `import` folder of your DBMS...
</details>

<details>
<summary>
Part 5: Let us play with some more data
</summary>

### Play with the 'Graph of Thrones' project data

If you would like to do a little more, there is a complete `Graph of Thrones` dataset that can be directly `played` by running `:play got` in the neo4j Desktop command line.

We want to import data from existing files. Let's `import` Characters of GOT book 1 from 2 local `.csv` files:

* book1edgessample.csv
  * this file has the following headers: `Source`, `Target`, `Type`, `id` &  `weight`
* book1gendersample.csv
  * it has the following headers: `Name`, `Nobility` & `Gender`

### Before we start...

Let's delete everything in our graph to make a fresh start:

```cy
MATCH (n)
DETACH DELETE n
```

⚠️ Remember: our nodes have relations (they are attached, so to speak), so we `DETACH` before we can `DELETE`:

Importing data from files means:

1️⃣ `LOADING` a `.csv` <br />
2️⃣ `CREATING` nodes & relations <br />
3️⃣ add `properties` upon creation

### Uniqueness constraints

Sometimes, we want to apply `uniqueness constraints`on properties that we want to be unique (like a unique identifier). Let us make sure that our nodes with a Character label do not have the same name property:

```cy
CREATE CONSTRAINT FOR (c:Character) REQUIRE c.name IS UNIQUE
```

We can check existing constraints in our graph with the following command:

```cy
SHOW CONSTRAINTS
```

Or with following buit-in function:

```cy
:schema
```

If necessary, we can also drop a constraint with its `name`:

```cy
DROP CONSTRAINT constraintName
```

### Load & check a `.csv` file

To import our first `.csv`, we need to load it:

1️⃣ place the `book1edgessample.csv` in the `import` folder <br />
2️⃣ `LOAD` it & pass it to a `variable` to make it available for manipulation

Let's first load the file and check whether neo4j can read it (using a `LIMIT` clause for efficiency):

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1edgessample.csv' AS line
RETURN line
LIMIT 10
```

⚠️ Instead of local files, we could use online files using an `http` address (ending by `.csv`)

### Load & create the data from `.csv`

`LOAD` does not import per se but makes the data available (using the column `headers` to reference where the information comes from). To avoid creating duplicate nodes, we use `MERGE` instead of `CREATE` (in our `.csv` file, Characters can appear either as Target or Source, so if we used `CREATE`, we would create them n-times):

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1edgessample.csv' AS row
MERGE (src:Character {name: row.Source})
MERGE (tgt:Character {name: row.Target})
MERGE (src)-[r:INTERACTS {weight: toInteger(row.weight)}]->(tgt)
```

The result should be `Added 100 labels, created 100 nodes, set 269 properties, created 169 relationships`

⚠️ Note the `toInteger()` statement that defines data from the 'weight' column as integers.

### Add data from another `.csv`

Let us view our graph:

```cy
MATCH (n)
RETURN n
```

For the moment, our nodes only have a `Character` label & a `name` property (this is what was in our `.csv` file). How can we import `properties` from another file?

1️⃣ add the `book1gendersample.csv` file to the import folder <br />
2️⃣ check whether it can be read

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1gendersample.csv' AS line
RETURN line
LIMIT 10
```

The file uses `Name`, `Nobility` & `Gender` as variables. When preparing the files, we made sure that names in the `Name` column are the same as in our first dataset (they have to match). We could try the following:

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1gendersample.csv' AS line
MERGE (c:Character {name: line.Name})
ON MATCH SET c.gender = line.Gender, c.house = row.Nobility
// Do not try this
```

This would work but it would create duplicates of Character nodes. We do not need the `MERGE` instruction: we do not create new nodes or relations:

```cy
LOAD CSV WITH HEADERS FROM 'file:///book1gendersample.csv' AS row
MATCH (c:Character {name: row.Name})
SET c.gender = row.Gender, c.house = row.Nobility
```

Let's check what this does:

```cy
MATCH (a:Character {name: 'Daenerys-Targaryen'}), (b:Character {name: 'Bran-Stark'})
RETURN a, b
```

### View the data structure with APOC & view the graph

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

### Analyze the data

We can now query the graph to analyze our data. We could also do simple stats (average & standard deviations): 

```cy
MATCH (c:Character)-[:INTERACTS]->()
WITH c, count(*) AS num
RETURN min(num) AS min, max(num) AS max, avg(num) AS avg_characters, stdev(num) AS stdev
// where count() counts the number of character nodes who have relations with other nodes
```

We could do much more, like centrality measures, shortest paths, pivotal nodes, etc.

### Let's do some counting

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
```

How many Character nodes have a `male` property?

```cy
MATCH (n:Character {gender: 'Male'}) 
RETURN count(n) AS numberOfMaleCharacters
```

Let's use a `WITH` clause to count Male & Female Characters & their ratio:

```cy
MATCH (m:Character {gender: 'Male'})
WITH count(m) AS numMales
MATCH (f:Character {gender: 'Female'})
WITH count(f) AS numFemales, numMales
RETURN numMales, numFemales, toFloat(numFemales) / numMales AS genderRatio
```

How many people does one character interact with?

```cy
MATCH (c1)-[r:INTERACTS]->(c2)
RETURN c1.name as name,
COUNT(*) AS Total
```

Can we count the number of interactions of one Character & get a list of the people they interact with? (`COLLECT`)

```cy
MATCH (c1)-[r:INTERACTS]->(c2)
RETURN c1.name as Person,
COUNT(*) as Total,
COLLECT(c2.name)
```

### Let's do some exploration

Let us identify: 

1️⃣ who interacts with Bran Stark besides Arya Stark  <br />
2️⃣ who interacts with both of them

...to do so, we need to identify the pattern that Cypher must `traverse` to return only the results we want:

```cy
MATCH (a:Character {name: 'Arya-Stark'})-[r1]-(b:Character {name: 'Bran-Stark'})-[r2]-(c:Character)
RETURN c.name
```

And:

```cy
MATCH (a:Character {name: 'Arya-Stark'})-[r1]-(b)-[r2]-(c:Character {name: 'Bran-Stark'})
RETURN b.name
```

### Ask questions

Who in the database has the same `gender` property as Arya Stark?

```cy
MATCH (c:Character), (c1:Character {name: 'Arya-Stark'}) 
WHERE c.gender = c1.gender AND id(c) <> id(c1)
RETURN c.name
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

### Let's practice...

Which Characters have a name that starts with 'J'?

```cy
MATCH (p:Character)
WHERE p.name STARTS WITH 'J'
RETURN p.name
```

### CASE clauses

We would like to export a dataset of the gender distribution by house to plot it (for instance in Python). To do this, we need a less simple clause: `CASE`, which introduces a condition (`WHEN`) & instructions:

```cy
MATCH (c:Character)
RETURN c.house AS House, 
COUNT(CASE WHEN c.gender='Male' THEN 1 END) AS NumberMales, 
COUNT(CASE WHEN c.gender='Female' THEN 1 END) AS NumberFemales, 
COUNT(CASE WHEN c.gender IS NULL THEN 1 END) AS NoGender;
```

### Let's ORDER our results

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

### Add dates

Dates are crucial in history, but the level of datation that we get from our sources can be very uneven and thus difficult to manipulate. 

Let's add a few (random) dates to our dataset by using the `date` data type: `date('YYYY-MM-DD')`:

```cy
MATCH (a:Character {name: 'Arya-Stark'})
MATCH (b:Character {name: 'Jon-Snow'})
SET a.dob = date('0300-09-23'), b.dob = date('0290-06-12')
```

Now, let's add another date property to a Character for which we just know the years of birth and death:

```cy
MATCH (c:Character {name: 'Bronn'})
SET c.dob = date({year: 295}), c.dod = date({year: 320})
RETURN c
```

Click on the node & check how the date was interpreted...

### Test the existence of properties

Let's test the existence of female Characters with a date of birth:

```cy
MATCH (p:Character)
WHERE p.gender = 'Female'
AND p.dob IS NOT NULL
RETURN p.name AS name, p.gender AS Gender, p.house AS Alliegence
```

Let's check relations that have a certain `weight` property:

```cy
MATCH (a)-[p]->(b)
WHERE p.weight IN [1, 5, 7]
RETURN a.name AS Name, p.weight AS Weight, b.name AS Target
```

### Play with dates

Bronn has a date of birth & a date of death. How old was he when he died?

```cy
MATCH (c:Character {name: 'Bronn'})
RETURN duration.between(c.dob,c.dod)
```

The result of the calculation is provided in `Cypher` notation. But we can use `year(s)` (and `day(s)` or `month(s)`) arguments: 

```cy
MATCH (c:Character {name: 'Bronn'})
RETURN duration.between(c.dob,c.dod).years
```

Who was born between the years 200 and 400?

```cy
MATCH (c)
WHERE c.dob IS NOT NULL
AND c.dob.year >= 200 <= 400
RETURN c.name As Character, c.dob AS Born
```

### Refactor

Let us add a second value to a property:

```cy
MATCH (c:Character {name: 'Jon-Snow'})
SET c.house = ['Stark', 'Nights-Watch']
RETURN c.name as Name, c.house as Houses
```

Look at the result and see how this created an array of values for the `house` property. Now, let's change a property name in bulk. We want to change `gender` to `sex` for all nodes, but keep the values:

```cy
MATCH (c:Character)
SET c.sex = c.gender
REMOVE c.gender
RETURN c
```

### Transform properties into nodes

Case: Our character nodes have a gender property but we want gender to be extracted and made into a specific node type instead of a property (we are thus changing the sctructure of our graph). We can do this in two steps (perhaps 1):

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

### Troubleshooting

* There is a lot on the neo4j website (see earlier)
* About temporal values, see: [https://neo4j.com/docs/cypher-manual/current/values-and-types/temporal/](https://neo4j.com/docs/cypher-manual/current/values-and-types/temporal/)
* Internet is your friend
* [Chat assistants are quite good at providing Cypher queries](https://platform.openai.com/playground)
</details>
