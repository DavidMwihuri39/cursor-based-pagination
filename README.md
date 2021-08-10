# Cursor-Based-Pagination
## Introduction
The pagination concept refers to breaking down large sets of data into small bits so that it can easily be useful to both the application and the user. A good example is when searching for something in Google. Google populates results on a page then breaks the results down to multiple pages depending on the keywords used. This is a breakdown of the multiple millions of results Google has found for the query. It breaks them down so that it is easy for the user to browse through (the process referred to as pagination or organizing data into pages). In pagination, users can move sequentially, from the first page through the pages all the way to the last page.
Pagination Example

<p align="center">
  <img width="220" height="110" src="https://github.com/DavidMwihuri39/cursor-based-pagination/blob/main/example_1.png">
</p>









## Limit/Offset pagination
At times, users face the challenge of fetching a huge number of records from a database. This causes a big problems especially in the cases where the records run into the thousands and cannot be queried by a simple select query. In this case, the concept of pagination is used where rather than taking records in large quantities at one go, the concept of taking limit and offset values to fetch records in small quantities and process them until all are retrieved from the database is used.
Cursor-based pagination
A cursor refers to a unique identifier for a definite record acting as a pointer to the following record the user wants to start querying to get the next results. When a cursor is implemented, the need to read rows that already been viewed is eliminated using a WHERE clause in the query which accelerates the process of reading data. This helps remove   inaccuracies in results by reading after a precise row rather than depending on the records position to be unchanged.
*Example* 
   
   ```
      Post
	|> order_by (inserted_at: :desc)
	
	|> limit (10)
	
	|> where [p], p.id , ^cursor
```
## Comparing Limit and Cursor based pagination
Though cursor based pagination is considered to be more effective, limit based pagination is the easier one to implement especially in cases where data is static. In addition, as limit based incorporate a mechanism of infinite scroll, it is considered efficient whereby the users can see most of the data at a go. 
Cursor based pagination in its part is more effective on its part as a result of various reasons. The very first of this is that instead of sending an offset parameter with the mannerisms of an index, this mode of pagination sends a cursor parameter which acts like a pointer to a specific record in the database indicating where the last page was set off ensuring no data is left out. In addition, one of the main advantages of this mode of pagination is its ability to perform data capabilities in real time. This is due to cursors not being tied down to static data hence new items can be manipulated without disturbing load procedures on each page.
## User Guide
It is time to get hands on and create a functional pagination example using the following technologies: GraphQL, Prisma, Photon/Lift, Nexus, Nexus-Prisma, and SQLite. Using these technologies, you can create a server with pagination running in less than 30 minutes. Here is a high-level overview of the steps that will be envolved in the process.

1. [Install Prisma](#1-install-prisma)
2. [Initialize Prisma Project and NPM Project](#2-initialize-prisma-project-and-npm-project)



### 1. Install Prisma

This installs Prisma in its entirity on your local. From this you can initialize everything you need to do with Prisma through the command line interface.

```
npm install prisma2 -g
```
### 2. Configure Prisma and NPM
Inside the project directory, run the command -- *following the prompt options listed*. This will call the Prisma Framework to open a project. A wizard like sequence will begin. Once Complete, you should now view a new directory called *pagination* inside the folder *Prisma*.
```
npx prisma2 init pagination-example
  - blank project
  - SQLite
  - CoffeeScript
  - Prisma schema
```

In addition, create package.json for the project. You will achieve this by running `npm init -y` inside the directory called *pagination*. The goal here is acting as an interface to add scripts and dependencies.

```
cd pagination-example
npm init -y
```
