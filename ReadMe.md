Advanced Replay Creator
==============================

This guide for my [Advanced Replay Creator](https://www.unrealengine.com/marketplace) teaches you everything to get started with creating a wide range of replays for your game. You can also find out what every function and exposed variable does!

Installation
------------
Coming Soon

The basics
----------
ARC works by recording frame data from **TrackedActors** and playing them back using **ReplayActors**. 

For example, a character transform would be recorded almost every Frame into an array. When the replay starts playing, the character would be hidden and a clone of it would be shown. The clone would update its transform using the recorded character transforms.

In this case the character would be the TrackedActor and the clone the ReplayActor. Also, from now on I'll call the hidden/shown as **killed**/**Waken** because it doesn't just hide the actors, it also removes collision, physics simulations and tick. More on that later.

 You control the whole replay by using a **ReplayComponent** and create blueprint instances of the **ReplayActor** base class with additional script using the built-in events and functions.


