Advanced Replay Creator
==============================

This guide for my [Advanced Replay Creator](https://www.unrealengine.com/marketplace) teaches you everything to get started with creating a wide range of replays for your game. You can also find out what every function and exposed variable does!

Installation
------------
Soon

The basics
----------
ARC works by recording keyframes from **TrackedActors** and playing them back with **ReplayActors**. 

For example, a character transform would be added multiple times a second into an array. When the replay would start playing, the character would be hidden and a "clone" of it would be shown. The clone would update its transform using the transform array.

In this case the character would be the TrackedActor and the clone the ReplayActor. Also, from now on I'll call the hidden/shown as **killed**/**Waken** because it doesn't just hide the actors, it also removes collision, physics simulations and tick. More on that later.

You control a replay with a **ReplayComponent** and create blueprint instances of the **ReplayActor** base class with additional script using the built-in events and functions.

Setting up a ReplayComponent
----------
Add a ReplayComponent to an easily accessable blueprint class such as PlayerState or GameMode. This way you can access it and control the replay anywhere.

(gif)

Before you start recording, you have to initialize the ReplayActors. The function for that is `AddReplayActors`.

(picture of the function)

It spawns ReplayActors for every inputted TrackedActor with correct child blueprints and settings.

 Input | Description
------------ | -------------
`ReplayActors` | Define the added ReplayActors' properties
`ReplayActorClass` | What ReplayActor child blueprint class are you using
`TrackedActors` | References for every tracked actor in the scene that replays with the class above
`AutoRecordTransform` | Automatically record and play the actor transforms for these ReplayActors

To start recording your replay, call `Record`.

(picture of the function)

 Input | Description
------------ | -------------
`Time` | At what point in replay-time you start recording (Leave the first time recording at 0)
`KillReplayActors` | Automatically kills all of the ReplayActors assigned to this ReplayComponent
`WakeTrackedActors` | Automatically wakes all of the TrackedActors assigned to this ReplayComponent

When your recording is ready and you want to play your replay, call `Play`.

(picture of the function)

 Input | Description
------------ | -------------
`Time` | At what point in replay-time you start playing
`Reversed` | Will the replay be played backwards
`KillTrackedActors` | Automatically kills all of the TrackedActors assigned to this ReplayComponent
`WakeReplayActors` | Automatically wakes all of the ReplayActors assigned to this ReplayComponent
