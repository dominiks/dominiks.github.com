.. title: Thinking about entity systems
.. slug: thinking-about-entity-systems
.. date: 2013/04/14 13:37:56
.. tags: games, draft
.. link: 
.. description: 

When working with an entity system one will sooner or later find oneself at a point where the technical questions and problems turn more into philosophical discussions. The original thought behind the entity system approach is to prevent hard coding any game object and represent them through a collection of components that are worked on by processors. This can be easily applied to classic game objects like npcs and the player or just static objects standing around in the world.

I use these terms like this:

Component
  An object containing some atomical data. A Component does not modify its data by itself.

Entity
  A reference to a set of components, there are no two components of the same type associated with an entity.

Processor
  Works with some components of specific entities and modifies their data.

But it gets a bit harder to grasp when thinking of things like the GUI, the actual game world itself or points. Are gui elements even a part of the entity system or can they be separated into something completely else? The game world - the current level - are some sort of singleton, are they still represented through a component or just a processor?

The game world
--------------

The game world is first and foremost **data**. Data that specifies the boundaries of the world and gives other entities a source of information to navigate through it. This leads naturally to the conclusion that we are dealing here with at least one component - the size of the world. 
