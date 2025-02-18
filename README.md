### Links

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)

### Entity-Component-System (ECS)

When developing games, a common problem every developer faces is how to define the "entities" within them. By "entities" I mean any object that you can find in a level -> rocks, items, characters, weapons, whatever. The issue is many entities in games share things in common - they might share 3D models, they might share physics properties, they might share audio and so on. The question is how can we share things in common between different types of objects while keeping the differences between them. In general it is good practice to make code reusable.

The traditional way to address this problem is to use an inheritance hierarchy. We define a base class called "AActor" containing all of the common functionality between entities (physics, 3D models, networking and so on). Every type of entity that we create can extend from this "base class" and add properties of its own that make it more unique. This is quite a common approach that is supported in many programming languages. The issue with inheritance is that it cannot be compartmentalized - either the entire thing is inherited or not at all. Many programming languages do not support multiple inheritance (you can only inherit from one thing at a time) and those that do often have name collision issues (the diamond problem). However, this approach is very straightforward and it has the nice property that any derived class you make can be used as templates for other objects.

A different approach used in many game engines is to use composition. Every entity is instead made up from components, each component is a structure that does a different thing and can be put together like lego to make more unique objects. For example, a player character might have an input component, a collider component and a mesh component. The collider and mesh component can be reused for other things, while the input component can remain on the player. This can be implemented using an array on each entity. Every component is contained within a slot in the array (using a pointer for each array element). You can mix and match different components together to create different entities. While straightforward and flexible, the indirection of composition like this can lead to performance problems, as each component is often placed in a haphazard manner in memory, which isn't great for the CPU cache (as I will explain).

ECS (Entity Component System) is a data-orientated approach that tries to achieve the flexibility of composition while improving overall performance, leveraging the underlying hardware. The issue is modern CPU's utilise a mechanism called "cache". Frequently used values is stored within the cache so that the CPU does not need to wait for the values to be retrieved all the way from RAM. Whenever a value is not in the cache, a "cache miss" occurs and the CPU needs to wait for the data to arrive. To mitigate this problem, modern CPU's use a hierarchy of caches: L1, L2 and so on. Each transfer between the cache levels is a cache line and each cache line is roughly 64 bytes, effectively moving chunks at a time. To further mitigate latency, the CPU also uses a "branch predictor". The branch predictor predicts the outcome of if-statements to fetch values to the cache ahead of time. ECS leverages these properties, storing the entity components next to each other (contiguously) so that they can be predictably fetched in sequence. So long as the components are next to each other and the same components iterated over in sequence, the branch predictor can predict the next components that will be needed and fetch them ahead of time. Further, since the components are stored next to each other, they can be transfered as part of the same cache line. To avoid poluting the cache with unwanted values, we split up the components into seperate arrays - one array for each component type. This way only the necessary components will be fetched for the operation at hand (cache space is very limited). 

ECS is a paradigm shift - instead of storing the components within the entities, the components are stored outside of the entities - one array for each component type. The entities are represented in ECS as an index into these arrays. For example, a player character could be represented by the index 0. Within each array, the component at index 0 would contain the values needed for that player. An ECS is made up of three parts: "Entities" are just index values or globally unique identifiers used to look up components, "Components" are the components stored within arrays, and "Systems" are functions or behaviours that iterate through the component types. Whereas an object orientated program would lump these features together into objects, ECS organises things differently to maximise cache performance. 

I've made my own ECS. To handle destroying entities it uses something called a "generational arena". Essentially, an entity is not directly an index into the components. Instead, the entity is first used to look up the index stored within an intermediary array. This intermediary array is needed so that requests for specific components can be redirected whenever the component is moved or destroyed. Each element within the array features a "generation number". Whenever an entity is destroyed, the corresponding generation number is incremented, so that requests for an older generation of entity can be safely discarded.
While an entity being "just an index" is a nice idea, in reality there needs to be some additional book-keeping to invalidate references properly when entities are destroyed. My ECS library also uses a technique called "archetypes" - you define the composition of your entities upfront so that it can use compile-time if-statements (if constexpr), making the code more branchless at runtime.

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

