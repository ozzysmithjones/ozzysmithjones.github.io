### Links

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)

### Entity-Component-System (ECS)

When developing games, a common problem every developer faces is how to define the "entities" within them. By "entities" I mean any object that you can find in a level -> rocks, items, characters, weapons, whatever. The issue is many entities in games share things in common - they might share 3D models, they might share physics, they might share audio et cetera. The question is how can we share things in common between different types of objects while keeping the differences between them. We should try to share commonality where it exists to avoid writing the same code over and over again. 

The traditional way to address this problem is to use an inheritance hierarchy. We define a base class called "AActor" containing all of the common functionality between objects (physics, 3D models, networking and so on). Every type of entity that we create can extend from this "base class" like a template and add properties that make it more unique. This is quite a common approach that is supported in many programming languages. The fundamental flaw with inheritance is that it cannot be compartmentalized - either the entire thing is inherited or not at all. Many programming languages do not support multiple inheritance (you can only inherit from one thing at a time) and those that do often have name collision issues with methods. However, this approach is very straightforward and it has the nice property that any derived class you make can be used as templates for other objects.

A different approach used in many game engines is to use composition. Every entity is instead made up from components. For example, a player character might have an input component, a collider component and a mesh component. The collider and mesh component can be reused for other things, while the input component can remain on the player. This can be implemented using an array on each entity. Every component is contained within a slot in the array (using a pointer for each array element). You can mix and match different components together to create different entities. While straightforward and flexible, the indirection of composition like this can lead to performance problems, as each component is often placed in a haphazard manner, which isn't great for the CPU cache (as explained later).

ECS (Entity Component System) is a data-orientated approach that tries to achieve the flexibility of composition while improving overall performance, leveraging the underlying hardware. The issue is modern CPU's utilise a mechanism called a "cache". Frequently used values is stored within the cache so that the CPU does not need to wait for the values to be retrieved from RAM. Modern CPU's use a hierarchy of caches L1, L2 and so on to mitigate overall latency. Each cache contains a limitied amount of memory that is used for frequently accessed data. While your program is running, the CPU will also use a "branch predictor" to predict the future outcome of if-statements and fetch values to the caches ahead of time. This means the more predictable the program is, the more efficient it will be. Each transfer between the caches is called a "cache line" and is roughly 64 bytes in size. Another observation is that data stored next to each other can be transfered as part of the same cache line and also lead to lower overall latency. ECS leverages these properties, storing the entity components next to each other (contiguously) so that they can be predictably fetched in sequence. So long as the components are next to each other and the same components iterated over in sequence, the branch predictor can predict the next components that will be needed and fetch them ahead of time. To avoid poluting the cache with unwanted data, only the relevant components are iterated through. 

ECS is a paradigm shift - instead of storing the components within the entities, the components are stored outside of the entities - one array for each component type. The entities are represented in ECS as an index into these arrays. For example, a player character could be represented by the index 0. 


I've made an Entity-Component-System for the game engine that I'm working on and I thought I would share how it works. 

The traditional approach of representing entities within games is to use an inheritance hierarchy. This approach has it's merits: it allows for reusing code while maintaining a clear hierarchy of objects. In the Unreal Game Engine, "Character" inherits from "Pawn" which inherits from "Actor". This relationship allows for reusable code found within the Actor class to be inherited by the pawn and character classes. The problem with inheritance is that the inheritance cannot be partial, it is not possible to "partially inherit" a class. All member variables must be inherited(copied) in their interity, even if they are unused in some of the derived classes. Another problem is that it can lead to the diamond problem: It can be difficult to create classes that need to be combination of two or more inheritance inheritance hierarchies. Multiple inheritance is generally not possible because the methods can conflict with each other. So, the advice generally used nowadays is to prefer "composition over inheritance" where classes are instead made of parts (sometimes called components) which can be reused in different circumstances. This allows for reusable code similar to an inheritance hierarchy but the reuse can now be "partial" - only reusing the features that are necessary. 

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

