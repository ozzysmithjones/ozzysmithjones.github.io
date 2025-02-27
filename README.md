### Links

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)

### Native Platform Layer
[Platform Layer](https://github.com/ozzysmithjones/platform_layer)

I've made a small platform layer library to wrap the windows API, you can find the library here: [Platform Layer](https://github.com/ozzysmithjones/platform_layer). My hope is to develop this library further
and make various projects with fewer dependencies.

### Entity-Component-System (ECS)
[Entity-Component-System](https://github.com/ozzysmithjones/entity-component-system) 
[Article](Articles/Entities.md)

When developing games, a common problem every developer faces is how to define the "entities" within them. By "entities" I mean any object that you can find in a level -> rocks, items, characters, weapons, whatever. The issue is many entities in games share things in common - they might share 3D models, they might share physics properties, they might share audio and so on. The question is how can we share things in common between different types of objects.

One way to address this problem is to use an inheritance hierarchy. We define a base class called "AActor" containing all of the common functionality between entities (physics, 3D models, networking and so on). Every type of entity that we create can extend from this "base class" and add properties of its own that make it more unique. This is quite a common approach that is supported in many programming languages. The issue with inheritance is that it cannot be compartmentalized - either the entire thing is inherited or not at all. Many programming languages do not support multiple inheritance (you can only inherit from one thing at a time) and those that do often have name collision issues (the diamond problem). However, this approach is very straightforward and it has the nice property that any derived class you make can be used as templates for other objects.

A different approach used in many game engines is to use composition. We define an entity class, and this entity class contains an array of "components". Each component is a discreet part that can be added or removed from the entity. You can mix and match different components together to create different things (a component in this case is just a class or data-structure). While straightforward and flexible, the indirection of composition like this can lead to performance problems. 

ECS (Entity Component System) is a data-orientated approach that tries to achieve the flexibility of composition while improving overall performance, leveraging the underlying hardware. ECS is a paradigm shift - instead of storing the components within the entities, the components are stored outside of the entities - one array for each component type. The entities are represented in ECS as an index into these arrays. For example, a player character could be represented by the index 0. Within each array, the component at index 0 would correspond to the values needed for that player. An ECS is made up of three parts: "Entities" are just index values or globally unique identifiers used to look up components, "Components" are data-structures stored within arrays, and "Systems" are functions or behaviours that iterate through the components. Whereas an object orientated program might lump these features together into objects, ECS organises things differently to maximise system performance. 

The issue is modern CPU's utilise a mechanism called "cache". Frequently used values is stored within the cache so that the CPU does not need to wait for the values to be retrieved all the way from RAM. Whenever a value is not in the cache, a "cache miss" occurs and the CPU needs to wait for the data to arrive. Modern CPU's use a hierarchy of caches: L1, L2 and so on. Each transfer between the cache levels is a cache line and each cache line is roughly 64 bytes. To mitigate latency, the CPU uses a "branch predictor". The branch predictor predicts the outcome of if-statements to fetch values to the cache ahead of time. ECS leverages these properties, storing the entity components next to each other (contiguously) so that they can be predictably fetched in order. So long as the components are next to each other and the same components iterated over in sequence, the branch predictor can predict the next components that will be needed and fetch them ahead of time. Further, since the components are stored next to each other, they can be transfered as part of the same cache line when transfers occur. To avoid poluting the cache with unnecessary values, we split up the components into seperate arrays - one array for each component type. This way only the necessary components will be fetched for the current operation (cache space is very limited). 

So ECS maximises performance by storing the components in seperate contiguous arrays. This is very different to the traditional object orientated approach, which would store the objects in different places and use polymorphism/dynamic dispatch (which is more unpredictable for the branch predictor). In the interest of trying ECS, I made my own header only ECS library. In addition to the technical details already discussed, it uses a generational arena to support destroying entities while invalidating references to those entities safely, and it uses a technique called "archetypes" where you specify the entity kinds ahead of time so the compiler can use compile-time if-statements (if constexpr) to generate branchless code. 

The problem with destroying entities is that in many games, entities often reference each other. For example, a zombie character might need to reference the player (as the zombie wants to eat their brain). Naively just using pointers is problematic - when the player's brain is eaten, that character will be destroyed, but the pointer will still point to it. There needs to be a mechanism, whereby whenever an entity is destroyed all references to it are invalidated. The simple way this is achieved in my library is to use an array, where each element corresponds to a specific entity. Each element in the array contains two properties: the index of the entity and the generation value. Whenever an entity is destroyed, it's corresponding generation value is incremented. This means that request for an entity from an older generation can be safely ignored and discarded. The index value is to support re-mapping entities. When an entity is destroyed, there is a hole left in memory that needs to be filled. My ECS moves the last element to fill in the hole, and redirects the index in the array to point there. All entity look ups first go through this array to validate things correctly. This lookup scheme allows the library to support destroying entities at runtime without problems. 

"Archetypes" solve a few distinct problems. When there's different kinds of entities, there needs to be a way to iterate through the different entity kinds without unnecessary if-statements/branches. Quite a common approach is to store just one array per component type, but this is not always practical. When there's different kinds of entities, some might not have all the components, leaving gaps in memory. To skip over the gaps there either needs to be more lookup (maybe a bitset per entity) and/or more branches (check if gap is empty with an if-statement). Instead of attempting to store all of the entity kinds together, the "archetype" technique is to make a seperate set of arrays per entity kind. My library uses this technique. The user of the library needs to provide the archetypes up front, so that the variadic templates can generate the approperiate code at compiletime to iterate through the different entities efficiently. 

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

