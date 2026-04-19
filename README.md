### Links

[Linkedin](https://www.linkedin.com/in/oscar-smith-jones-44329a195/) 
[Twitter](https://twitter.com/OscarSmithJone1)
[GitHub](https://github.com/ozzysmithjones)

## Bounding Index values at Compile-Time
Simple way to validate index values at compile-time. First time I saw this approach I felt that the idea should be more widespread.
Article: [Bounding Indices at Compile Time](Articles/BoundsChecking.md)

### Error Handling

Thoughts on error handling in programming languages and the problems that they don't solve but really should:
Article: [Error Handling](Articles/ErrorHandling.md)

### Spear Programming Language

Thoughts/Design ideas for a programming language that imposes minimal mental burden on the programmer:
(I know, tried many times, this one is different I swear).
Article: [Spear](Articles/Spear.md)

### Entity-Component-System (ECS)

[Entity Component System](https://github.com/ozzysmithjones/entity-component-system) 

ECS library. This library is used to represent objects in video games in a much more data-orinetated way, such as characters, items, enemies and so on. 
Good for avoiding cache-misses in performance-critical code and a different paradigm to Object Orientated Programming.
article: [Entities](Articles/Entities.md) 

### Vulkan Renderer

[Vulkan Renderer](https://github.com/ozzysmithjones/LearnVulkan)

A demonstration of using the Vulkan API to make a 3d model rotate on the screen. 

### Chess AI

[Chess AI](https://github.com/ozzysmithjones/Chess)

Exploration of plenty of algorithm optimizations to make a good chess bot. It is wildy known that AI is good at playing chess,
but not many might know of all the smart ideas to get a good one working.

## Game Engine From scratch in C

[C Game Engine](https://github.com/ozzysmithjones/gameoverlord)

Very Minimal Game Engine that only uses six c./.h files to implement the back-end. It can do sprite-batching, hot-reloading, play sounds, manage user input and draw graphics to the screen.
Intended to be used as a game framework where you just download the project and extend from the game as a template (this makes managing the hot-reloading feature simpler). A full game-engine might
use a custom build-system and project initialization tool (like Unreal does), who knows.
