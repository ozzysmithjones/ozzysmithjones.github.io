C++ Programmer and Game Developer. 

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)
## Portfolio
### Entity-Component-System (ECS)

[Entity-Component-System](https://github.com/ozzysmithjones/entity-component-system)

An Entity-Component-System is a data-orientated approach for representing entities within games. The idea is to structure entities in a way to mitigate cache misses, which is a common source of performance problems within games. I made my own personal version of ECS with an archetype-based approach. This version uses C++ variadic templates to provide an intuitive API and it uses `if constexpr` to further filter candidate items at *compile-time*. It is designed for projects with high-performance requirements and object counts, like Real-Time-Strategy games. 

This project was inspired by Unity DOTS, which has an ECS system: [Unity Dots](https://unity.com/dots). Also I was inspired by the Entt library, which is currently used commercially in Minecraft and many other projects: [Entt](https://github.com/skypjack/entt) 

