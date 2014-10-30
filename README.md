# play-heroku-seed README
You can try a deployed instance of the app here: https://afternoon-wildwood-5782.herokuapp.com/

This application is meant to make it easier to create a Play application with basic Create, Read, Update and Delete functionality and get it up onto Heroku. Database manipulation is handled by Slick connected to PostgreSQL. 

## Getting started

### Local development on OSX
#### Database Setup
- Install PostgreSQL 9.3 Postgres.app is the easiest way: [postgresapp.com](http://postgresapp.com/)
- Set up local databases matching the settings in config/database.yml
- [PostgreSQL full documentation](http://www.postgresql.org/docs/9.3/interactive/)
- Run `psql`
- Set up a development database, note the underscores:
`CREATE DATABASE play_heroku_seed;`
- Confirm the database has been created by looking at the database list
`\l'


#### Run the application
- In the root folder of the repo:
`activator run`
- In a browser, open localhost:9000. If no folder for conf/evolutions/default exists, do not worry. On first request in browser, slick will automatically inspect the models, and generate a 1.sql file in conf/evolutions/default. This initial file is a complete schema of the application.

#### Development
##### Common database tasks
- If you need to connect to the database to inspect it or run sql: 
`\c play_heroku_seed;`
and if you are currently logged into osx as "johnsmith" you should see
`You are now connected to database "play_heroku_seed" as user "johnsmith".`

- View users created in the user table by securesocial:
1. Open psql, connect to the database:
`\c play_heroku_seed;`
2. View user table data: 
`SELECT * FROM "user";` 

Note: "user" is also a keyword in PostgreSQL, if you enter this command without quotation marks, it will not select from the play_heroku_seed user table, but instead will output from PostgreSQL's internal database users table and you will get something like this:

```
play_heroku_seed=# SELECT * FROM user;
 current_user 
--------------
 Mashallah
(1 row)
```

- To reset your local database:
`DROP DATABASE play_heroku_seed;`
`CREATE DATABASE play_heroku_seed;`
and run the application


##### Introducing model changes to the database
If you modify the models, and you do not care about current production data (still before launch):

1. Stop the application
2. Delete conf/evolutions/default/1.sql
3. Open psql, reset the database by doing the following:
`DROP DATABASE play_heroku_seed;`
`CREATE DATABASE play_heroku_seed;`
4. Run the application, visit localhost:9000

A 1.sql file reflecting the current state of the application models will be auto-generated by slick, auto-applied by play, and now running. If a 1.sql file was not generated, you have likely introduced a change to the model that slick cannot interpret.

Slick is currently unable to generate incremental database evolution files to make those changes. It can only generate a complete snapshot of the application models at any point. If you want to introduce incremental changes to the models, you will need to manually write the SQL database evolutions.

### Requests for contribution
- Optimal Procfile. Heroku's web process is disabled when using the procfile in Play 2.3.5 documentation. The app currently does not include a Procfile, instead relying on Heroku's defaults and Heroku's parsing of application.conf. If you have a good Procfile, please pull request. 