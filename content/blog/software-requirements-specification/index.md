---
title: Software Requirements Specification - A living document
date: "2021-02-28T23:46:37.121Z"
---

# -> Dish & Dance <-
# Table of Contents
1.  [App User Flow] (#user-flow)
## User Flow
1. User lands on app
2. User is able to see list of recipes, for example 5 recipes
3. User is view a playlist for a recipe
4. User is able to like a playlist a recipe
5. User is able to vote on songs in a playlist
6. App is able to sort songs in a playlist in order of total number of likes

Think of decoupling as a good practice

We want to describe how our software system should be developed <sup id="a1">[1](#f1)</sup>
  

Browser
  - client
Server
  - web server hosting files
  - python
    - createRecipes()
      - makes a call to the mysql DB to insert records
  - golang
    - backend server for our app 
    - getSongs(:playlistId)
      - makes a call to the mongoDB to get records
Database
  - read/write
    - with different software, think of the latency, execution time
  - agnostic, doesn't what language or server requirements
  - data
  - mysql
    - app - chef who creating recipes
  - mongodb
    - good for reading
    - app - general users who aren't logged
      - they can listen
  - cron jobs
    - every 1 hour, pull data from mysql, and push to mongodb
    - replication


1. application receives a POST request
2. store the content of this POST request in its memory.
3. application state will consist of the data sent inside the request.
4. After that, you may decide to persist (keep) the state of your application. 
   1. you can write the current application state inside a database.
5. Another request will load data again, either from from the database


establish a API contract
- server and the database
- database
  - song_id
- front-end
  - developer created the UI
    - songID
- server
  - developer
    - songId

User
  - vote on a song
    - POST
      - persist the vote on the song for the user, so when they login the next, the vote is in the application state
  - like a playlist
  - name
  - favorites
    - GET
      - from the response, we store the values (from the database) in the application state
  - functions
    - voteOnSong(:songId, value)
    - voteOnPlaylist(:playlistId, value)
  - models
    - name
    - favorites
App
   - view recipes
   - functions
     - login
   - models
     - recipes
     - playlist
     - song
     - user
Recipes
  - view playlist
  - name
  - functions
    - viewRecipe(:id)
    - createRecipe()
    - deleteRecipe(:id)
  - models
    - name
    - 
Playlist
  - collection of songs
  - number of likes
  - sort by number of likes
  - functions
    - viewPlaylist
    - addPlaylistToRecipe(:recipeId, :playlist)
    - removePlaylistFromRecipe(:recipeId, :playlist)
  - models
    - name

For each of the following will have functions and data models associated with them:

- App
- User
- Recipe
- Playlist
- Song

### Data Model

| Data Model  | Name        | Description |
| ----------- | ----------- | ----------- |
| App | `app` | The application |
| Recipe| `recipe`| A recipe can hold information about a playlist|
| Playlist | | A playlist is a list of songs |
| Playlist |`playlistId` | playlistId of a playlist |
| Playlist |`title` | title of a playlist |
| Playlist |`userId` | userId of a playlist |
| Playlist |`totalNumberOfLikes` | totalNumberOfLikes of a playlist |
| Song| | A song data model|
| Song| `title`| title of song |
| Song| `description`| description of song |
| Song| `length`| length of song |
| Song | `title` | Title of the song |
| Song | `youtubeURL` | A URL reference for YouTube |
| Song | `imageURL` | A URL reference for image |
| Song | `numberOfVotes` | The total number of votes |
| User | `user` | The user that is logged in |


### Functions

|Data Model | Name        | Description |
| ----------- | ----------- | ---| 
| app | `login`      | Handles authentication and authorization of user       |
| song | `addSongToPlaylist(:songId, playlistId)`      | Add a song to a playlist       |
| song | `removeSongToPlaylist(:songId, playlistId)`      | Remove a song from a playlist       |
| playlist | `voteOnPlaylist(:playlistId, vote)`      | Vote on a playlist        |
| playlist | `createNewPlaylist` | Add a new playlist |


## Data Flow

How the data is handled in the system. You have a set of handlers (functions) that change the state of the data. `f(x) = x ^ 2`

`Browser --> UI/UX --> API --> Server --> Database`

Browser
- handle user events
  - clicks

UI/UX 
- create user-friendly
  - where should the vote button be?
  - pagination

API 
  - CRUD
    - `/api/playlist/13?vote=-1`

Server 
  - Handle the api request and map to appropriate service
  - Service will connect to the DB and execute the script

Database
  - The data models that we created are for creating the tables in the database
  - single source of truth
  - Update the data

```
Table: Song
songId  playlistId  title             singer        
23      13          Intha Porapudan   Kailash Kher

Table: Playlist
playlistId  title               userId    totalNumberOfLikes
13          Kajan's Favourites  1004      5
```


### User Flow

1. User lands on app
2. User is able to see list of recipes, for example 5 recipes
3. User is view a playlist for a recipe
4. User is able to like a playlist a recipe
5. App is able to sort playlist for a recipe in order of like count
6. User is able to vote on songs in a playlist

#### Data Flow
When the user clicks on the vote down button.

Browser will a API call, POST request `/api/playlist/13?vote=-1`

The server will receive the request. There are routes that map an API to a `Service::Function`, follows MVC pattern (Model View Controller)

```
Server Routes to Services
{
  playlist: {
    'GET': PlaylistService::GetPlaylist(playlistId),
    'POST' PlaylistService::UpdatePlaylist(playlistId)
  }, 
  login: {
    POST: UserService:login
  }
}
```

The server will need to process the request and delegate to the appropriate handler. 

`PlaylistService` - manages all playlist related functionality

`PlaylistService::voteOnPlaylist(:playlistId, vote)` - The function that will update the `vote` field based on the `playlistId`

`voteOnPlaylist(13, -1)`

```javascript
// Expose some functions to update the database
import DatabaseService

function voteOnPlaylist(playlistId, vote) {

  DatabaseService.connect()

  DatabaseService.executeScript(
    `
      UPDATE playlist
      SET totalNumberOfLikes = totalNumberOfLikes + ${vote}
      WHERE playlistId = ${playlistId}
    `
  )
}
```

```
Table: Playlist
playlistId  title               userId    totalNumberOfLikes
13          Kajan's Favourites  1004      4
```

1. App is able to sort songs in a playlist in order of total number of likes


