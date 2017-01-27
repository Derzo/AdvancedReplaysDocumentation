Advanced Replay Creator
==============================

This guide for my [Advanced Replay Creator](https://www.unrealengine.com/marketplace) teaches you everything to get started with creating a wide range of replays for your game. You can also find out what every function and exposed variable does!

Installation
------------
Soon

The basics
----------
ARC works by recording frame data from **TrackedActors** and playing them back using **ReplayActors**. 

For example, a character transform would be recorded almost every Frame into an array. When the replay starts playing, the character would be hidden and a clone of it would be shown. The clone would update its transform using the recorded character transforms.

In this case the character would be the TrackedActor and the clone the ReplayActor. Also, from now on I'll call the hidden/shown as **killed**/**Waken** because it doesn't just hide the actors, it also removes collision, physics simulations and tick. More on that later.

You control a replay by using a **ReplayComponent** and create blueprint instances of the **ReplayActor** base class with additional script using the built-in events and functions.

Getting Started; ReplayComponent
----------
Add a ReplayComponent to an easily accessable blueprint class such as PlayerState or GameMode. This way you can access it and control the replay anywhere.

Before you start recording, you have to call `AddReplayActors`.
(picture of the function)
It spawns ReplayActors for every inputted TrackedActor with correct child blueprints and settings.

> **ReplayActors**: Define the new ReplayActors' properties
> 
> **ReplayActorClass**: What ReplayActor child blueprint class are you
> using
> 
> **TrackedActors**: References for every replayed tracked actor that uses
> the class above
> 
> **AutoRecordTransform**: Automatically record and play the actor
> transforms for these ReplayActors

To start recording your replay, call `Record`.
(picture of the function)

**Time**: At what point in replay-time you start recording (Leave the first time recording at 0)

**KillReplayActors**: Automatically kills all of the ReplayActors assigned to this ReplayComponent

**WakeTrackedActors**: Automatically wakes all of the TrackedActors assigned to this ReplayComponent

 Input | Purpose
------------ | -------------
Time | At what point in replay-time you start recording (Leave the first time recording at 0)
KillReplayActors | Automatically kills all of the ReplayActors assigned to this ReplayComponent
WakeTrackedActors | Automatically wakes all of the TrackedActors assigned to this ReplayComponent
