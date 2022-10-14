## Description
---
This repo provides the ETL pipeline, to populate the sparkifydb database. 
In this project, I'll apply what I've learned on data modeling with Postgres and build an ETL pipeline using Python. To complete the project, I will need to define fact and dimension tables for a star schema for a particular analytic focus, and write an ETL pipeline that transfers data from files in two local directories into these tables in Postgres using Python and SQL.
* The purpose of this database is to enable Sparkify to answer business questions it may have of its users, the types of songs they listen to and the artists of those songs using the data that it has in logs and files. The database provides a consistent and reliable source to store this data.

* This source of data will be useful in helping Sparkify reach some of its analytical goals, for example, finding out songs that have highest popularity or times of the day which is high in traffic.

## Database Design and ETL Pipeline
---
* For the schema design, the STAR schema is used as it simplifies queries and provides fast aggregations of data.
## Schema for Song Play Analysis
# Fact Table
* songplays - records in log data associated with song plays i.e. records with page NextSong songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
# Dimension Tables
* 1 - users - users in the app: user_id, first_name, last_name, gender, level

* 2 - songs - songs in music database: song_id, title, artist_id, year, duration

* 3 - artists - artists in music database: artist_id, name, location, latitude, longitude

* 4 - time - timestamps of records in songplays broken down into specific units: start_time, hour, day, week, month, year, weekda

![Schema](schema.PNG)

* For the ETL pipeline, Python is used as it contains libraries such as pandas, that simplifies data manipulation. It also allows connection to Postgres Database.

* There are 2 types of data involved, song and log data. For song data, it contains information about songs and artists, which we extract from and load into users and artists dimension tables

* Log data gives the information of each user session. From log data, we extract and load into time, users dimension tables and songplays fact table.

## Running the ETL Pipeline
---
* First, run create_tables.py to create the data tables using the schema design specified. If tables were created previously, they will be dropped and recreated.

* Next, run etl.py to populate the data tables created.

## etl.py File Contains:
* process_song_file Function This function can be used to read the file in the filepath (data/song_data)
  to get the song and artist info and used to populate the songs and artists dim tables.
* process_log_file Function This function can be used to read the file in the filepath (data/log_data)
  to get the user and time info and used to populate the users and time dim tables.
* process_data Function get all files matching JSON extension from directory

## Create_tables.py File Contains:
* create_database Function: Creates and connects to the sparkifydb and Returns the connection and cursor to sparkifydb
* drop_tables Function: Drops each table using the queries in `drop_table_queries` list.
* create_tables Function:  Creates each table using the queries in `create_table_queries` list.

## sql_queries.py File Contains:
* list of postgresql Queries that Create or Drop all tables and insert data into the table