# Migration 

## Database Steps

```sql
CREATE TABLE "lift"."User" (
  "email" TEXT NOT NULL DEFAULT ''  ,
  "id" TEXT NOT NULL   ,
  "name" TEXT    ,
  PRIMARY KEY ("id")
);

CREATE TABLE "lift"."Post" (
  "author" TEXT    REFERENCES "User"(id) ON DELETE SET NULL,
  "content" TEXT    ,
  "createdAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  "id" TEXT NOT NULL   ,
  "published" BOOLEAN NOT NULL DEFAULT false  ,
  "title" TEXT NOT NULL DEFAULT ''  ,
  "updatedAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  PRIMARY KEY ("id")
);

CREATE UNIQUE INDEX "lift"."User.id" ON "User"("id")

CREATE UNIQUE INDEX "lift"."User.email" ON "User"("email")

CREATE UNIQUE INDEX "lift"."Post.id" ON "Post"("id")
```

## Changes

```diff
diff --git datamodel.mdl datamodel.mdl
migration ..20191114170035-initial
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,25 @@
+datasource db {
+  provider = "sqlite"
+  url      = "file:dev.db"
+}
+
+generator photon {
+  provider = "photonjs"
+}
+
+model User {
+  id    String  @default(cuid()) @id @unique
+  email String  @unique
+  name  String?
+  posts Post[]
+}
+
+model Post {
+  id        String   @default(cuid()) @id @unique
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  published Boolean
+  title     String
+  content   String?
+  author    User?
+}
```

## Photon Usage

You can use a specific Photon built for this migration (20191114170035-initial)
in your `before` or `after` migration script like this:

```ts
import Photon from '@generated/photon/20191114170035-initial'

const photon = new Photon()

async function main() {
  const result = await photon.users()
  console.dir(result, { depth: null })
}

main()

```