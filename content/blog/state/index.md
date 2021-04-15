---
title: The state of the union
date: "2021-04-15T00:07:45Z"
---

## State - What is it?

In any given point in time, there is a different information stored in the memory of your web application that you can access via your variables, classes, data structures, etc. All the stored information, at a given instant in time, is called the application state.

How would you represent the state when the chip is full, new?
  isOpen = false
  weight = 40g

How would you represent the state when the chip is empty, finished?
  isOpen = true
  weight = 0g

If we are trying to access a single resource (chipBag), we would need to append an id to the path. 

For example: 
```
GET olddutch.com/chipbags/:id — retrieves the chipbag in the chipbags resource with the id specified. 
  {
    name: "Old Dutch"
    flavour: "Ketchup"
  }

DELETE olddutch.com/chipbags/:id — deletes the chipbag in the chipbags resource with the id specified.
```

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

```
LOG
2021-03-12 POST /water/drink?amount=400
cookie: __cfduid=d92f0a26b87ed4fd1697eabc8a857bf341615756644
2021-03-13 POST /water/drink?amount=400
cookie: __cfduid=d92f0a26b87ed4fd1697eabc8a857bf341615756644
2021-03-12 POST /water/drink?amount=4000000
cookie: __cfduid=d92f0a26b87ed4fd1693434328a857bf341615756644
10 reqs/hour
```

- Resources are the nouns of the Web - they describe any object, document, or thing that you may need to store or send to other services.

Because REST systems interact through standard operations on resources, they do not rely on the implementation of interfaces.

These constraints help RESTful applications achieve reliability, quick performance, and scalability, as components that can be managed, updated, and reused without affecting the system as a whole, even during operation of the system.


---