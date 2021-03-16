# User Flow
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




In any given point in time, there is a different information stored in the memory of your web application that you can access via your variables, classes, data structures, etc. All the stored information, at a given instant in time, is called the application state.

How would you represent the state when the chip is full, new?
  isOpen = false
  weight = 40g

How would you represent the state when the chip is empty, finished?
  isOpen = true
  weight = 0g

If we are trying to access a single resource (chipBag), we would need to append an id to the path. 

For example: 

GET olddutch.com/chipbags/:id — retrieves the chipbag in the chipbags resource with the id specified. 
  {
    name: "Old Dutch"
    flavour: "Ketchup"
  }

DELETE olddutch.com/chipbags/:id — deletes the chipbag in the chipbags resource with the id specified.
---

- Systems that follow the REST paradigm are stateless
  - meaning that the server does not need to know anything about what state the client is in and vice versa. 
  - agnostic
  - both the server and the client can understand any message received, even without seeing previous messages.
    - in the now, no concept of past 
- This constraint of statelessness is enforced through the use of resources, rather than commands. 
- Example: go running
  - Observer: Sarmilan
    - water bottle
        - volume: 500ml
        - isFull: true
    - my left road is in front of my right
    - i've stopped running
    - tying shoelace
    - drank water
      - state > POST /water/drink?amount=400 > new state
      - water bottle
        - volume: 100ml
        - isFull: false
    - ran 5km
    - tied shoes
    - drank water
      - enforced this using a resource (object representation)
      - water bottle
        - volume: 50ml

LOG
2021-03-12 POST /water/drink?amount=400
cookie: __cfduid=d92f0a26b87ed4fd1697eabc8a857bf341615756644
2021-03-13 POST /water/drink?amount=400
cookie: __cfduid=d92f0a26b87ed4fd1697eabc8a857bf341615756644
2021-03-12 POST /water/drink?amount=4000000
cookie: __cfduid=d92f0a26b87ed4fd1693434328a857bf341615756644
10 reqs/hour


- Resources are the nouns of the Web - they describe any object, document, or thing that you may need to store or send to other services.

Because REST systems interact through standard operations on resources, they do not rely on the implementation of interfaces.

These constraints help RESTful applications achieve reliability, quick performance, and scalability, as components that can be managed, updated, and reused without affecting the system as a whole, even during operation of the system.


---

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

For each of the following will have functions and data models associated with them

## Introduction

### Purpose

### Intended Audience

### Intended Use

### Scope

### Definitions and Acronyms

##  Overall Description

### User Needs

### Assumptions and Dependencies

## System Features and Requirements

### Functional Requirements

- App
- User
- Recipe
- Playlist
- Song

#### App

##### Data Model

| Name        | Description |
| ----------- | ----------- |
| `recipe`      | A recipe can hold information about a playlist       |
| `playlist`   | A playlist is a list of songs        |

##### Functions

| Name        | Description |
| ----------- | ----------- |
| `login`      | Handles authentication and authorization of user       |

#### Song

##### Data Model

| Name        | Description |
| ----------- | ----------- |
| `title` | Title of the song |
| `youtubeURL` | A URL reference for YouTube |
| `imageURL` | A URL reference for image |
| `numberOfVotes` | The total number of votes |

##### Functions

| Name        | Description |
| ----------- | ----------- |
| `addSongToPlaylist(:songId, playlistId)`      | Add a song to a playlist       |
| `removeSongToPlaylist(:songId, playlistId)`      | Remove a song from a playlist       |

### External Interface Requirements

### System Features

### Nonfunctional Requirements



# Next Steps
- [ ] Create outline
- [ ] Create list of questions
- [ ] Clean up markdown file and push to remote repo



# Resources
- <b id="f1">1</b> [Your Guide to Writing a Software Requirements Specification (SRS) Document](https://relevant.software/blog/software-requirements-specification-srs-document) [↩](#a1)