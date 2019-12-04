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

Generasting the map is only step one, next ill be looking to create empire influence and spaceships. Afterwards ill be implementing A* pathfinding and player input. 


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
