# Sparkify Songplays ETL-Pipeline

## Project Description
This project's scope is to read two types of JSON files; the first file contains various information about songs and the second containing the Sparkify user log's. An ETL-pipeline will then load this information into fact and dimension tables on a PostgreSQL database for further analysis. 

## The ETL-Pipeline
A Python script will read all the JSON files within in the song_data and log_data directories. The Python scripts then import the data into two dataframes. The data will be written to the database into various fact and dimension tables in the last step.

## The database schema
The purpose of this database is to analyze the user logs and see what songs are played. Also, it should be possible to further enrich this data by looking up specific artists, users, and song names.

To allow fast reading of the song plays, and the ability to filter and enrich on specific concepts, this database is modeled by using a star schema with fact and dimension tables. The database contains the following tables:

### songplays (fact table)
This table allows for fast reading of the history of the song plays by the Sparkify users,  it has foreign keys to the various dimension tables for further enrichment by song, artist, or user.

### users (dimension table)
Here we store the Sparkify users and some of their profile information.

### artists (dimension table)
A list of all artist and their location of the songs available in Sparkify.

### songs (dimension table)
All the songs with a link to their corresponding artist

### time (dimension table)
This table contains a breakdown of the timestamp stored in the songplay table in hour, week, month, year, and weekday.

## Example queries
Find sons by title, artist name and song duration
```SQL
SELECT s.song_id, s.artist_id 
FROM songs s 
JOIN artists a ON s.artist_id = a.artist_id
WHERE s.title = %s AND a.name = %s AND s.duration = %s
```

See what users played the most songs the last 30 days
```SQL
SELECT user_id, count(*) AS songplays
FROM songplays
WHERE timestamp > (CURRENT_TIMESTAMP - 30)
GROUP by user_id
```
