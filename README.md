C++ Programmer and Game Developer. 

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)
## Portfolio
### Entity Component-System (ECS)

[Entity Component-System](https://github.com/ozzysmithjones/entity-component-system)

An Entity-Component-System is a data-orientated approach for representing entities within games. The idea is to mitigate the occurance of "cache misses", an event that occurs when the data that is needed by the CPU cannot be found in the CPU cache. By mitigating this source of latency, ECS allows for games to efficiently process larger quantities of game objects at higher rates. 
I made one version of this idea, except this implementation also uses `if constexpr` to further filter candidate items at *compile-time*.  It is designed for projects with high-performance requirements and object counts, like Real-Time-Strategy-Games. 

This project was inspired by Unity DOTS, which has an ECS system: [Unity Dots](https://unity.com/dots). Also I was inspired by the Entt library, which is currently used commercially by Minecraft and many other projects: [Entt](https://github.com/skypjack/entt) 

