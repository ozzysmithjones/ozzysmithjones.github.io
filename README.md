C++ Programmer and Game Developer. 

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)

## Portfolio

### Entity-Component-System (ECS)

I've made an Entity-Component-System for the game engine that I'm working on and I thought I would share how it works. 

An Entity-Component-System is a data-orientated approach for representing objects within video games. This is designed to be more performant and flexible than the traditional Object-Orientated techniques. The traditional approach of representing entities within games is to use an inheritance hierarchy. The Unreal Game Engine uses an inheritance hierarchy where "Character" derives from "Pawn" which derives from "Actor". This approach has it's merits: you can reuse code found in the parent class within the derived classes, and you can override certain behaviours to change what the object does. The problem is, all code and variables within a parent class must be inherited by a derived class in it's entirety, the inheritance cannot be partial. This can lead to bloat where there's unused variables in a parent class that is still present in the derived class. This approach can also lead to the diamond problem, where it can be difficult to create classes that need to be a combination of multiple inheritance hierarchies. So, generally the advice used nowadays is to prefer "composition over inheritance" where objects are instead made of parts (or components) that can be reused in different places. This allows reusable code similar to an inheritance hierarchy but the reuse can now be partial - only the data and code that is needed can be added to the new object. 

ECS takes this composition approach one step further by storing the components in contiguous arrays. An entity in this version can just be represented as an index into these arrays. Storing the components in contiguous arrays is relevant for performance: regions of memory are often transfered between CPU caches in chunks, called cache lines, which are generally 64 bytes in size. So, by storing the components next to each other, it is likely that multiple components will lie within the same chunk, effectively mitigating the total number of transfers that need to made. Also, since the systems within the game will generally process these components in order without branches, the branch predictor (which is located on the CPU) can fetch the components that follow ahead of time. This is essentially the idea behind Data-Orientated-Programming: model the data structure as it is most effective for the CPU architecture as opposed to modeling the data structure as a real-world object (such as in Object Orientated Programming). 

Inspired by this idea, I've made my own header-only library to try ECS in my own projects. In this implementation, you declare the entity kinds yourself with `EntityArchetype`. The library also uses `if constexpr` to filter these archetypes at *compile-time* for the operations that you do with them. The library can be found here: 
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

