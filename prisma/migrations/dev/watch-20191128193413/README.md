# Migration `watch-20191128193413`

This migration has been generated by Joseph Ospina Tafur at 11/28/2019, 7:34:14 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "lift"."User" (
  "email" TEXT NOT NULL DEFAULT ''  ,
  "id" TEXT NOT NULL   ,
  "name" TEXT    ,
  PRIMARY KEY ("id")
);

CREATE TABLE "lift"."Post" (
  "author" TEXT NOT NULL   REFERENCES "User"(id) ON DELETE RESTRICT,
  "content" TEXT    ,
  "createdAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  "id" TEXT NOT NULL   ,
  "published" BOOLEAN NOT NULL DEFAULT false  ,
  "title" TEXT NOT NULL DEFAULT ''  ,
  "updatedAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  PRIMARY KEY ("id")
);

CREATE UNIQUE INDEX "lift"."User.email" ON "User"("email")
```

## Changes

```diff
diff --git datamodel.mdl datamodel.mdl
migration ..watch-20191128193413
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,26 @@
+datasource db {
+  provider = "sqlite"
+  url      = "file:dev.db"
+  default  = true
+}
+
+generator photon {
+  provider = "photonjs"
+}
+
+model User {
+  id    String  @default(cuid()) @id
+  email String  @unique
+  name  String?
+  posts Post[]
+}
+
+model Post {
+  id        String   @default(cuid()) @id
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  published Boolean  @default(false)
+  title     String
+  content   String?
+  author    User
+}
```

## Photon Usage

You can use a specific Photon built for this migration (watch-20191128193413)
in your `before` or `after` migration script like this:

```ts
import Photon from '@generated/photon/watch-20191128193413'

const photon = new Photon()

async function main() {
  const result = await photon.users()
  console.dir(result, { depth: null })
}

main()

```
