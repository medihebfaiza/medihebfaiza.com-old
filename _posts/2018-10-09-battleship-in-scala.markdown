---
layout: post
title:  "Battleship in Scala"
date:   2018-10-09 12:01:02 +0100
categories: projects
---

<img src="https://i.imgur.com/yP2GHgy.png" alt="main image"/>

## Description 
This is a very simple and minimalistic classic Battleship game written in scala. It can be played between two players or against AI with three different levels.

The rules of the game are the same as the classic one with the possibilty to play multiple rounds and therefore every player has a score.

To proove that every AI is superior in intelligence to the one before it, the game has a proof mode that runs three different matches with 100 rounds each between the three levels of AI and records the results on a csv file.  

## How to Play
### Human VS Human
1. Choose mode 0 from the game menu
2. Enter the number of game rounds
3. Each Player must place 5 ships with different sizes on his primary grid and therefore must enter an initial position (ex : 0A) and a direction (horizontal or vertical) for each ship.
4. Each turn a Player must enter a target coordinates meaning a row number and a column letter
5. Each round the grids are reset and each Player must re-place his fleet.  

### Human VS AI
1. Choose mode 1 from the game menu
2. Enter a difficulty level from 0 to 2 (0 being Beginner level and 2 being Hard level)
3. Place your fleet
4. Enter the target cell coordinates each turn

### AI VS AI (Proof)
1. Choose mode 2 from the game menu
2. Let the game run until it stops
3. Check the proof results in the "ai_proof.csv" file on the root folder 

|AI Name     | score | AI Name2  | score2 |
------------ | :---: | --------- | :-----:  
|AI Beginner | 47    | AI Medium | 53     |
|AI Medium   | 3     | AI Hard   | 97     |
|AI Beginner | 1     | AI Hard   | 99     |

## Architecture
To guide the design process, my priority was to respect and apply the different design principles that i have learned during my training like divide and conquer, increase reusability and design for portability and testability.

The Architecture that i found most suited for this application and that respects most of these design principles is the Component Based Architecture.

In this architecture, the application is decomposed into multiple reusable functional components that can be divided into two categories : 
- Business logic Components : these are the business process entities that are implemented in scala as immutable data structures using case classes.
- Infrastructure Components : these are components that have specific responsibilities like display and data persistence and they are implemented in scala using objects.

### Pros
- Single Responsibility Principle :
Each component has a specific responsibility that helps in it’s integration with other components.

- High Cohesion :
There is no overlapping among the components functionality and therefore we have a separation of concerns.

- Abstraction : 
Components depend on abstracted components and not the concrete components. Example : Whether it is a Human or an AI the GameState Component only depends on the Player abstraction. 

- Testability : 
Each Component can be tested separately.
### Cons
- High Coupling : 
Updating  the smallest unit of composition like a Cell for example implies updating all other components containing it in our case the Grid or the Ship then the player then the GameState. I call this The Matryoshka doll effect.
In Addition, storing Ship cells and grid cells separately requires keeping them synchronized whenever a updated occurs.

- Low reusability : 
Some components are designed to be reused in different situations in different applications. However, most of the components are context specific. They are designed for the specific context of this application.

![alt text](https://i.imgur.com/pEn8VdI.png "Architecture")


## AI
### AI0 : The Random One
The first AI uses  random play. It shoots at positions that are randomly generated and it doesn’t keep track of the shot cells so it can shoot a cell multiple times.
This AI makes poor effort and takes too much time to finish a round as the majority of cells should be shot in order to insure that all the ships are sunk.

Speaking of randomness, here’s a random joke :

![alt text](https://i.imgur.com/lJ7LuSg.gif "Joke")

Credits : [Scott Adams](http://dilbert.com/)
### AI1 : The Dummy with a Memory
The second AI uses a similar strategy to the first one. It’s targets are generated randomly but it avoids shooting at the same cell more than once by using a tracking grid. Every time it shoots a cell, it marks it as hit or missed on its tracking grid. Then, every time it is asked for a target it generates a random one while making sure it was not shot before.

### AI2 : The Hunter
Like the second AI, the third AI keeps track of the shot cells. In addition, this AI “hunts” for cells by stacking the potential targets surrounding a hit cell.
Each time a cell is hit, it adds the surrounding “non shot yet” cells to a set then it starts hunting. The next turn, when it’s asked for a target it gives a target that was popped out from the set.
If the set of potential targets is empty, it goes back to search mode and generates a random target each time until it hits a new cell and so on.

## Post-Mortem
Before this project my knowledge of Functional Programming was minimal and working on it was a great opportunity for me to understand more about its principles and apply what I have learned during the courses.

At the beginning of the project, I took the time to plan before starting to write the code. I wanted to write high quality code and to follow the scala style guide lines so I went through a process of careful thinking and research. I didn’t plan too much though but just enough to have a clear vision of the project and I left room for change. The fact that I have previously written a similar game in python during my training helped me gain intuition and time.
During development I made sure I was using the right tools. At the beginning I refused to use the IntelliJ IDE but then I decided to get out of my comfort zone and try it and it turned out to be a great IDE. 
Scala build tools, Scalatest and Scaladoc are also great tools that definitely helped me boost my productivity. Not to mention Git, Trello and Travis CI.

On the other hand, I can’t say that everything went perfectly fine during this project. 
Coming from a C++, Python and Java background, It took me some time to get used to applying the Functional Programming principles. I have written many functions in Imperative Programming that I had to transform to Functional Programming afterwards. This process was a bit of a hassle and took me longer than I thought.
In addition, using an IDE from the beginning would have saved me much more time.
Some features of the application need more improvement like for example placing the fleet for AI uses brute force and this can be improved by implementing an algorithm that places the ships intelligently. The AI can be also improved by implementing more heuristics.
The application code maybe very minimalistic but that’s just probably a matter of me trying always to keep the code simple and avoiding writing features that I might not need in the future.

In the end, I have learned that the foundations of any project are the most important part and they must be done the right way before moving to other features. Therefore, I should have respected the Functional Programming principles from the beginning. 
I have also learned that in programming coding is only 10% percent of the process and the rest is all about reviewing, planning and researching.
Finally, I would like to mention that this project has required a certain organisation, discipline and consistency in the efforts made.

## Scala Style Check 
- [x] Avoid usage of Any & Nothing
- [ ] Avoid asInstanceOf & isInstanceOf
- [ ] Avoid usage of Monads
- [x] Avoid usage of return
- [ ] Avoid option.get
- [x] Avoid usage of null
- [x] Avoid usage of while and for
- [x] Check for boolean expressions that can be simplified
- [ ] Check that functions do not define mutable variables
- [x] Check the number of lines in a file

## Author
@medihebfaiza
