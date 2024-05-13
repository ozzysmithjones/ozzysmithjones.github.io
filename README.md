### Links

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)

### Entity-Component-System (ECS)

I've made an Entity-Component-System for the game engine that I'm working on and I thought I would share how it works. 

The traditional approach of representing entities within games is to use an inheritance hierarchy. An inheritance hierarchy is a hierarchy where classes can completely inherit the member variables and methods of other classes. The Unreal Game Engine uses an inheritance hierarchy where "Character" derives from "Pawn" which derives from "Actor".  This allows for members that are common to all "Actors" to also be shared across with all characters within the game. The problem with inheritance is that all members within a parent class must be inherited by the derived classes in it's entirety, it is not possible to "partially inherit" a class. This approach can lead to bloat, since there may be members in the parent class that are unused in some of the derived classes. This approach can also lead to the diamond problem, where it can be difficult to create classes that need to be a combination of multiple inheritance hierarchies. So, generally the advice used nowadays is to prefer "composition over inheritance" where objects are instead made of parts (or components) that can be reused in different places. This allows reusable code similar to an inheritance hierarchy but the reuse can now be partial - only the data and code that is needed can be added to the new object. 

ECS takes this composition approach one step further by storing the components in contiguous arrays. An entity in this version can just be represented as an index into these arrays. Storing the components in contiguous arrays is relevant for performance: regions of memory are often transfered between CPU caches in chunks, called cache lines, which are generally 64 bytes in size. So, by storing the components next to each other, it is likely that multiple components will lie within the same chunk, effectively mitigating the total number of transfers that need to made. Also, since the systems within the game will generally process these components in order with fewer branches, the branch predictor (which is embedded in hardware) can predict the code that will execute and fetch the components that follow ahead of time. This is essentially the idea behind Data-Orientated-Programming: model the data structure as it is most effective for the CPU architecture as opposed to modeling the data structure as a real-world object (such as in Object Orientated Programming). 

ECS is an acronym and is made of three parts: Entity, Component, System. `Entities` are generally identifiers or indices used to associate components within seperate arrays to form game objects that can be found in the level. ECS stores each `Component` type in a seperate array, so that only the components within the cache-line will be the ones used for the operation at hand. `Systems` process the components by looping through them and performing some transformations on them. To make this structure work, there's still some problems that need to be addressed by the particular implementation.

One problem is how to handle different kinds of entities with different sets of components. There's some trade-offs that can be made here when coming up with a solution. One approach is to use a bitset per entity that describes what components it has. The issue here is that this introduces more branches into the code, this unpredictability of the code can make the branch prediction mechanism less effective. Another approach is to use the archetype pattern: create different contiguous arrays for different entity kinds. This alternative approach eliminates the branch prediction problem by dividing the entity kinds into groups, but it introduces more allocations into the code and the memory is less contiguous. For my own implementation, I decided to go with the archetype approach. 

Another problem is the issue of handling destroyed entities: If entity A (say a zombie) is following entity B (say a player) how to handle entity A in this situation when the entity B is destroyed? If this event is not handled correctly it could lead to unexpected behaviour or a crash within the game. The way I went by solving this problem is to use a generational arena - instead of using entity/component indices or pointers directly, an id or "generation" value is used to first validate that the entity is still alive before acquiring a pointer/index. If the entity is destroyed, it's corresponding generation value is increased, so that the next time the entity is requested the followers record of the entity generation is invalidated(as it is an old value) and the request can be safely dropped and handled appropriately. 

My ECS library can be found here: 
[Entity-Component-System](https://github.com/ozzysmithjones/entity-component-system) 

This project was inspired by Unity DOTS, which has an ECS system: [Unity Dots](https://unity.com/dots). Also I was inspired by the Entt library, which is currently used commercially in Minecraft and many other projects: [Entt](https://github.com/skypjack/entt) 

### Vulkan Renderer

[Vulkan Renderer](https://github.com/ozzysmithjones/LearnVulkan)

Vulkan is an API used to interface directly with the GPU on a computer. Recently I developed a project that renders a 3D model directly to the screen using only the Vulkan SDK and the GLFW library. Game engines often depend upon an API such as Vulkan to present 3D models to the screen, so learning this tech is very handy for future game engine projects. 

### Chess AI

[Chess AI](https://github.com/ozzysmithjones/Chess)

Sometimes your programming journey takes you to a rabbit hole, one such rabbit hole is chess AI. During my time at University, I made a chess AI that can beat a 1600 rated opponent. At the time this AI was much better at playing chess than I was. There's many AI techniques used in this project to filter inadequate moves and make predictions of better moves from previous moves. 

### Tower Defence Genetic Algorithm

[Tower Defence Genetic Algorithm](https://github.com/ozzysmithjones/GeneticAlgorithm)

A genetic algorithm is the technique of using theories of evolution to develop an AI gradually over time. This partivular genetic algorithm finds the optimal sequence of towers to place in a tower defence game, their locations and the order that the towers should be placed in. This is an old project but might be interesting never the less.

