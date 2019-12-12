## Oscar Smith-Jones - games programmer

Oscar, C++, C# programmer studying at staffordshire University. Here are some of my projects.Some of which may still be in development.

[linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[gitHub](https://github.com/ozzysmithjones)

![profile](https://github.com/ozzysmithjones/ozzysmithjones.github.io/blob/master/ProfileJPG.jpg)


# Turn based Strategy game.

![Image](https://github.com/ozzysmithjones/ozzysmithjones.github.io/blob/master/GalaxyGeneration.PNG?raw=true)

4X stands for Explore,Expand, Exploit and Exterminate. 

## Procedural generation

You are looking at a procedurally generated galaxy, created using the Unity game engine. Every circle represents a star system, with the lines representing the different paths a spaceship can make from there. Over 600 stars are generated, the web is all connected, no star or collection of stars is disconnected from the galaxy. 

### How it works. 

This procedural generation system uses a breadth-first search. The algorithm starts with one star in the center. Every iteration, the algorithm picks one star and randomly generates the stars connecting to it. Since it's a breadth-first search, it prioritises the stars that are the fewest steps from the begining, maintaining a mostly circular map. 

In previous projects, the stars were randomly placed, the connections between them generated afterwards. This meant that some collection of stars wouldn't be connected to the rest of the galaxy. Worse still, the algorithm used to connect the stars together was performance intense; Every star needed to search the nearby area with a mini breath-first-search to connect the stars together. 

### Project


### next stage in the project

So the reason I am making a procedurally generated galaxy is beacuse I have been really interested in AI for strategy games lately. The recent Google deepmind AI  _AlphaStar_ has managed to defeat proffesional human players at the strategy game _StarCraft_. This project inspired me to try creating an AI of my own, but on a smaller scope with a turn based system on a node-based map. I have also played the classic _Master of Orion_ and I have played some _stellaris_ and _Civilisation_. Hopefully, I can make my own strategy game using this map that I created.

creating the map is only step one, next ill be looking to create empire influence and spaceships. Afterwards ill be implementing A* pathfinding and player input. 

# Rouge-Like

![RougeLike](https://github.com/ozzysmithjones/ozzysmithjones.github.io/blob/master/FIRE.PNG?raw=true)

## Dystopia 

The above shows a small rougelike project made in UE4 during my education years in college. We had three months to make a game and document about it every day. I was partnered with an Audio specialist, and we decided at the time a rougelike would best suit our skills and shared interests. Honestly, without an artist it wasn't easy making the background art, we went through two iterations of artwork before deciding on a very minimalistic design. The project was quite ambitious but we managed to finish it on time.

Dystopia is a Rougelike set underground, there's Aliens, wurms and strange diseases that can infect the player. Some Items are cursed, such as a god living within an Item or a laser gun that makes you age. 

## Development

With a fixed time of three months, me and one sound designer had to create a video game. We had one theme: Climate. For a small games jam, a restricting theme helps you to come up with an idea, even if that idea wasn't very good. But, for a 3 month game, it was quite hard to come up with something that we liked, that fit the theme, and showed our skills. After a week of thinking, we decided to make a rougelike beacuse: it was technical enough, A lot of varied audio could be created,and with enough style it could fit the theme. We envisioned this dark, gloomy rougelike with a nuclear holocaust theme, what could go wrong, right? 

We spent the next week planning and gathering research. What we didn't fully realise was that we also had to market to an audience. This game was going to be marked by the college, so we spent all of our time doing market research and a style guide for points. Tom never did any planning beacuse "he just didn't need one" ,apparently, so I had to help him plan out his work, given how long it takes him to complete a song, or sound effect. 

The next few weeks were especially difficult. Given the way I planned the project, the most important tasks(procedural generation, pathfinding, AI, inventory systems) were also the most difficult. These had to be done at the start. This meant that the artwork had to be done by the sound specialist, who worked with me in the past. Every day I would work into the night improving the dungeon generation, or making the pathfinding in _visual scripting_ while Tom would give me art assets. Regular meet-ups and blog posts would be made about meeting milestones. I had to learn to be very patient, but very vigilant with work over a long period. 

Eventually I had one bug, this one bug involved a crash on startup, but I couldn't find the source of the bug. I had to re-code the procedural dungeon generation, and fix the pathfinding system several times before it was resolved. this meant that we missed a couple of weeks of development. Luckily, I planned 4 weeks spare in-case of events like this. Even when everything worked, I looked at the game we created and realised that it was terrible. 

The artwork was atrocituous, no artist on team, most of our artwork came from the sound specialist. The main mechanic(using food as a resource) was badly balanced with the abilities currently in the game. The enemies provided no interesting decision making, beacuse they were too simple. Everything was working but playing atrocioustly. The game obviously needed several weeks of polish, but we didn't have several weeks. With market research, I also realised that rougelikes were an already crowded market, so if I was making this game for real, it would not succeed.

Despite the hit of low-motivation, I re-planned the middle of my project and worked as diligantly as I could. creating an inventory system, player progression and even curses. The game right now is alright, it has a lot of flaws, but the themes pretty good. If I could do this again- I would never do this again. Still, I learned great teamwork skills and general project experience.


## How it works

The enviroment is a procedurally generated dungeon. Two A* pathfind algorithms are used for the characters to navigate the enviroment. The first A* algorithm finds what rooms the moving character needs to pass through to reach their target. The second algorithm is restricted by what rooms it can navigate to by the first A* algorithm. The game also uses state-driven enemy AI, the player has an Inventory system, can gain multiple new abilities, and there's traps(fire) that fill the enviroment.   

Most of these systems make use of Unreal engines "Actor Component". An "Actor Component" can be added to an object in Unreal and removed later on. This makes it a useful tool for creating Inventory systems, Ability systems, traps, ect. The game also used OOP for organisation, inheritence and code efficiency. There are a few set parent classes in this project: Enemy, Item, ActionEffect, Trap, each with behaviour that is inherited from the child classes used in the game. An Array on the player contains all of that players Items, which can be of varying classes since they all inherit from the Item class. 

The dungeon enviroment is inspired from the game _Spelunky_ it's generated in a similar method to that game. In Spelunky, a random starting Tile is selected near the top of a 4 by 4 grid, this is the start of the procedural generation.Each step, spelunky picks a random neighbouring tile to the previous location and generates a room there. If the algorithm reaches an edge, then the next room is downwards. Eventually, spelunky generates a path of rooms reaching the bottom corner of the grid, at which point the end of the level is placed. Every room is generated using a template designed by the developers with a few random differences created by the game. In Dystopia, the enviroment is created in a similar method, except branching paths are also created starting from the main path, which travel until meeting a deadend, at which point they merge with the rooms already there. The templates used for the rooms in the dystopian game decide how much and what kind of food, enemies and items does each room have in the game. Since there isn't much foliage or objects in the game, the templates aren't as detailed as spelunkies. With more time, a more in-depth procedural generation method could of been used, perhaps even experimenting with AI like _Caves Of Qud_. 

Every tile in the game has a single Integer representing it's coordinates. These single-Integer coordinates are converted regularly into 2 dimensional coordinates for ease of use. Some abilities, like teleporting, needed to show one or more tiles that can be selected on the map. The enemies with these abilities, shouldn't show optional targeting tiles on the map. The Abilities UI behaviour is called from the players HUD, this method marks the tiles that can be selected. Only if the player selects a marked tile does the ability activate. Thus, the Ability behaviour is seperate from it's UI behaviour, allowing enemies to use the ability without showing any "select" icons on the map.  The only issue is that enemies need to have a target selected before using the ability, but by default this is set to the players location in a "attack" ability.

# Links to these projects. 
Links to these projects will be posted on GitHub. 





