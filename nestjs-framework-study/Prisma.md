# Prisma ORM with Nest.js

## Viewing the DB Data without PostgreSQL PgAdmin
When we have prisma, we don't really need the PgAdmin to view and do some editing to the data tables
Run the below command will open the Prisma Studio at `http://localhost:5555`:
```bash
npx prisma studio
```
If your schema and prisma migrations are all in order there will be no error here (show on the above web page)

## Making Changes to Schema & Migrations
- The data schema of the DB are defined in the `prisma/schema.prisma` file. 
- Whenever there is a change to the data schema (new col or new table), the schema file is updated and the change is registered and replayed to the DB via a migrations:
```bash
npx prisma migrate dev --name <name of migration>
```
- To undo or remove some of the migrations, we can roll-back to a migration by name:
```bash
npx prisma migrate resolve --rolled-back <migration_name>
// eg: 
npx prisma migrate resolve --rolled-back "20201127134938_added_bio_index"
```
- **(Not Recommended)** In case you want to reset all the previous migrates (**CAUTION** this *will* erase all data) you can run the migration reset command:
```bash
 npx prisma migrate reset
```