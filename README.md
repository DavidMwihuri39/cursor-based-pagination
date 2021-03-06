# Cursor-Based-Pagination
## Introduction
The pagination concept refers to breaking down large sets of data into small bits so that it can easily be useful to both the application and the user. A good example is when searching for something in Google. Google populates results on a page then breaks the results down to multiple pages depending on the keywords used. This is a breakdown of the multiple millions of results Google has found for the query. It breaks them down so that it is easy for the user to browse through (the process referred to as pagination or organizing data into pages). In pagination, users can move sequentially, from the first page through the pages all the way to the last page.

*Pagination Example*

<p align="center">
  <img width="220" height="110" src="https://github.com/DavidMwihuri39/cursor-based-pagination/blob/main/example_1.png">
</p>


## Limit/Offset pagination
At times, users face the challenge of fetching a huge number of records from a database. This causes a big problems especially in the cases where the records run into the thousands and cannot be queried by a simple select query. In this case, the concept of pagination is used where rather than taking records in large quantities at one go, the concept of taking limit and offset values to fetch records in small quantities and process them until all are retrieved from the database is used.

## Cursor-based pagination
A cursor refers to a unique identifier for a definite record acting as a pointer to the following record the user wants to start querying to get the next results. When a cursor is implemented, the need to read rows that already been viewed is eliminated using a WHERE clause in the query which accelerates the process of reading data. This helps remove   inaccuracies in results by reading after a precise row rather than depending on the records position to be unchanged.

*Example* 
   
   ```
      Post
	|> order_by (inserted_at: :desc)
	
	|> limit (10)
	
	|> where [p], p.id , ^cursor
```
## Comparing Limit and Cursor Based pagination
Though cursor based pagination is considered to be more effective, limit based pagination is the easier one to implement especially in cases where data is static. In addition, as limit based incorporate a mechanism of infinite scroll, it is considered efficient whereby the users can see most of the data at a go. 
Cursor based pagination in its part is more effective on its part as a result of various reasons. The very first of this is that instead of sending an offset parameter with the mannerisms of an index, this mode of pagination sends a cursor parameter which acts like a pointer to a specific record in the database indicating where the last page was set off ensuring no data is left out. In addition, one of the main advantages of this mode of pagination is its ability to perform data capabilities in real time. This is due to cursors not being tied down to static data hence new items can be manipulated without disturbing load procedures on each page.
## User Guide
Using the information above, we can now create a functional pagination example using the following technologies: GraphQL, Prisma, Nexus, Nexus-Prisma, and SQLite. Using these technologies, you can create a server with pagination running in less than 30 minutes. Here is a high-level overview of the steps that will be involved in the process.

1. [Install Prisma](#1-install-prisma)
2. [Configure Prisma](#2-Configure-prisma)
3. [Configure NPM](#3-configure-NPM)
4. [Add Project Scripts](#4-add-project-scripts)
5. [Install Dependencies](#5-install-dependencies)
6. [Create Apollo Server](#6-create-apollo-server)
7. [Generating Schema through GraphQL Nexus](#7-generating-schema-through-graphql-nexus)


### 1. Install Prisma

This installs Prisma in its entirity on your local. From this you can initialize everything you need to do with Prisma through the command line interface.

```
npm install prisma2 -g
```
### 2. Configure Prisma
Inside the project directory, run the command -- *following the prompt options listed*. This will call the Prisma Framework to open a project. A wizard like sequence will begin. Once Complete, you should now view a new directory called *pagination* inside the folder *Prisma*.
```
npx prisma2 init pagination-example
  - blank project
  - SQLite
  - CoffeeScript
  - Prisma schema
```

### 3. Configure NPM
The third step is to create package.json for the project. You will achieve this by running `npm init -y` inside the directory called *pagination*. The goal here is acting as an interface to add scripts and dependencies.

```
cd pagination-example
npm init -y
```

### 4. Add Project Scripts

In your package.json, project scripts are essential as they are needed to run the Appolo Server and generate the database access client. This is done the first time as subsequent attempts once installation is complete are autogenerated after installation.

```js
"dev": "node ./index.js",
"postinstall": "prisma2 generate"
```

### 5. Install Dependencies

Next, you need to install the required dependencies for the project. **Nexus-prisma** is essential as it provides unique features such as **pagination**.

```
npm install mine                   # this is the custome node.js server
npm install apollo-server-mine     # this is the custom graphql mine server
npm install nexus                     # this is creation of the code-first graphql schema
npm install prisma-nexus              # customary bindings for both prisma and nexus
npm install graphql                   # this is installation of graphql
```
### 6. Create Apollo Server
Build the standard Apollo Server without any special functionalities. Use the code-first approach with GraphQL Nexus to create the schema.

```js

const mine = require('mine');
const { ApolloServer } = require('apollo-server-mine');



const server = new ApolloServer({ });

const app = mine();
server.applyMiddleware({ app });

app.listen({ port: 5000 }, () =>
  console.log( Server launched at http://localhost:5000${server.graphqlPath}`)
);
```
### 7. Generating Schema through GraphQL Nexus

Building the code should be the first thing undertaken when building the schema. Begin with the mandatory imports these being nexus and nexus prisma. Nexus helps you create types of GraphQL and schema. Nexus prisma on the other hand acts as an intermediary between nexus and the prisma environment. 

```js

const { objectType, queryType, makeSchema } = require("nexus")
const { nexusPrismaPlugin } = require('nexus-prisma')
const { join } = require('path')
```
      
Using JavaScript, add the various types you plan on exposing in the API. From the code source shown, we handle *objectType* used in User and Post. The function is to expose the identifier fields mostly email address for Users for Posts publicly.

```js 

const User = objectType({
  name: 'User',
  definition(t) { t.model.id(), t.model.email() }
})
const Post = objectType({
  name: 'Post',
  definition(t) { t.model.id(), t.model.title(), t.model.author() }
})
```

Start exposing queries through the use of *queryType* in turn building a root Query. The pagination environment is now ready for implementation after combining Nexus-Prisma. 
```js 

const Query = queryType({
  definition(t) {
    t.crud.posts({ pagination: true })
  }
});
```

Using your types, build a schema through nexus-prisma's plugin an outputs object and *makeSchema*. This combines all of the types above into a GraphQL Schema.

```js

const schema = makeSchema({
  types: [Query, User, Post],
  plugins: [nexusPrismaPlugin()],
  outputs: {
    typegen: join(
      __dirname,
      '../node_modules/@types/nexus-typegen/index.d.ts',
    ),
  },
});
```
