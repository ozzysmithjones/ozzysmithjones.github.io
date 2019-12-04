## Oscar Smith-Jones - games programmer

Oscar, C++, C# programmer studying at staffordshire University. Here are some of my projects.Some of which may still be in development.

You can use the [editor on GitHub](https://github.com/ozzysmithjones/ozzysmithjones.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.


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


## How it works

The enviroment is a procedurally generated dungeon. Two A* pathfind algorithms are used for the characters to navigate the enviroment. The first A* algorithm finds what rooms the moving character needs to pass through to reach their target. The second algorithm is restricted by what rooms it can navigate to by the first A* algorithm. The game also uses state-driven enemy AI, the player has an Inventory system, can gain multiple new abilities, and there's traps(fire) that fill the enviroment.   

Most of these systems make use of Unreal engines "Actor Component". An "Actor Component" can be added to an object in Unreal and removed later on. This makes it a useful tool for creating Inventory systems, Ability systems, traps, ect. The game also used OOP for organisation, inheritence and code efficiency. There are a few set parent classes in this project: Enemy, Item, ActionEffect, Trap, each with behaviour that is inherited from the child classes used in the game. An Array on the player contains all of that players Items, which can be of varying classes since they all inherit from the Item class. 

The dungeon enviroment is inspired from the game _Spelunky_ it's generated in a similar method to that game. In Spelunky, a random starting Tile is selected near the top of a 4 by 4 grid, this is the start of the procedural generation.Each step, spelunky picks a random neighbouring tile to the previous location and generates a room there. If the algorithm reaches an edge, then the next room is downwards. Eventually, spelunky generates a path of rooms reaching the bottom corner of the grid, at which point the end of the level is placed. Every room is generated using a template designed by the developers with a few random differences created by the game. In Dystopia, the enviroment is created in a similar method, except branching paths are also created starting from the main path, which travel until meeting a deadend, at which point they merge with the rooms already there. The templates used for the rooms in the dystopian game decide how much and what kind of food, enemies and items does each room have in the game. Since there isn't much foliage or objects in the game, the templates aren't as detailed as spelunkies. With more time, a more in-depth procedural generation method could of been used, perhaps even experimenting with AI like _Caves Of Qud_. 

Every tile in the game has a single Integer representing it's coordinates. These single-Integer coordinates are converted regularly into 2 dimensional coordinates for ease of use. Some abilities, like teleporting, needed to show one or more tiles that can be selected on the map. The enemies with these abilities, shouldn't show optional targeting tiles on the map. The Abilities UI behaviour is called from the players HUD, this method marks the tiles that can be selected. Only if the player selects a marked tile does the ability activate. Thus, the Ability behaviour is seperate from it's UI behaviour, allowing enemies to use the ability without showing any "select" icons on the map.  The only issue is that enemies need to have a target selected before using the ability, but by default this is set to the players location in a "attack" ability.


```markdown
Syntax highlighted code block

# Header 1 
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ozzysmithjones/ozzysmithjones.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
