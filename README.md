![](https://img.shields.io/badge/category-Big--Data-success)
![](https://img.shields.io/badge/Program-Cassandra-green)
![](https://img.shields.io/badge/resource-Open-blue)

# BigData-Cassandra_CQL
Demo of Apache Cassandra 2.1, to demonstrate the usage and features of CQL.

- Installed and run distributed NoSQL database management system Apache Cassandra cluster with a single node on Windows10.  
- Use CQL (Cassandra Query Language) to create and change tables and write queries to find various information through the Cassandra shell.  


## Application

This data model is for a fictitious video sharing web site. The features we'll be supporting are:

 - User creation
 - Video file upload with metadata
 - Lookup of videos per user
 - Lookup of videos by tag
 - Comments on each video
 - Lookup of all comments made a particular user
 - Playback tracking per video with the intent of resuming video where they left it.

## Description of tables

- **users** - Simple static entity table to store data about a person. Uses a List collection to store more than one email per person.

- **videos** - A static table to store metadata about an uploaded video. Each video has an owner and the username is stored to reference back. A Map collection is used to store the location of the video files. This could be used to store various geographic locations if a CDN was in use. Tags are a Set collection and consist of one-word descriptions of the video.
- **username_video_index** - A one-to-many example linking users to videos. The lookup is based on username and optionally upload_date and videoid. Videoid is added to allow for more than one video with the same name. The clause "WITH CLUSTERING ORDER BY (upload_date DESC)" has been added to reverse sort the order in that videos are stored based on date. Video records for each user will be stored from the latest uploaded to the oldest.

- **video_rating** - Example of a counter table. rating_counter stores how many times the video was rated. rating_total is the cumulative count of rating points per video. The application can then take rating_total and divide it by rating_count for the average rating.
- **tag_index** - Index table to create a lookup of videos based on a unique tag. Tags are added to each video, but each tag may have multiple videos. This table allows that kind of lookup.
- **comments_by_video and comments_by_user** - Two tables that create two separate, but related, the one-to-many relationship between users and comments on videos. Each video has many comments. Each user has many comments. The "WITH CLUSTERING ORDER BY" clause is used on each table to keep the latest comments at the beginning of the storage row.
- **video_event** - Example of a time series data model where there can be an unknown amount of records. This example stores each video playback event (START, STOP) with the video timestamp for player advancement and the time of the actual event. From this table, we can derive where the user left off with the video and where to start them again. We can also determine if users are watching an entire video or how many times they viewed a particular video.


## References
- [Installing Apache Cassandra on Windows in 2021](https://www.youtube.com/watch?v=hJxlkHafYsQ)
- [Installing Apache Cassandra 4 on Linux](https://www.youtube.com/watch?v=wezbMP1uBkU)
- [Developing with Cassandra on Windows](http://www.luketillman.com/developing-with-cassandra-on-windows/)
- [How to Install Cassandra on Windows 10](https://phoenixnap.com/kb/install-cassandra-on-windows)
- [How to Create, Drop, Alter, and Truncate Tables in Cassandra](https://phoenixnap.com/kb/create-drop-alter-and-truncate-tables-in-cassandra)
- [Sample Cassandra CQL Schema](https://github.com/pmcfadin/killrvideo-sample-schema)
