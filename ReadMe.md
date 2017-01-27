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

Before you can start recording, you have to initialize the ReplayActors. The function for that is `AddReplayActors`.

(picture of the function)

It spawns ReplayActors using the inputs:

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

If you want to stop recording **or** playing, call `Stop`.

(Picture of the function)

That should do it for the basics of using a ReplayComponent.

Setting up the ReplayActors
---------------------------
First you need to think what actors you want to replay. The whole scene? Not a problem. Split them to different classes, such as a class for moving static mesh platforms and a class for the character.

You only need to do it for actors that will interact with something. For example, it would be a waste to create a ReplayActor class for a sleeping hamster animation actor that doesn't have AI and will never wake up. Or even if it does have AI, it's still a hamster that no-one will really their eye on. Just let the poor hamster live through the whole replay process.

To create a ReplayActor blueprint, right click, Create New Blueprint and select the class ReplayActor.

(gif)

Open it up and you'll find nothingness. To add looks, add components. Most usual case would be to bring the static- and skeletalmeshcomponents from the TrackedActors into the ReplayActor. If you know their meshes & materials will be constant, you can set them up there, or if they can change between instances, use the function `CopyMeshAndMaterials` on BeginPlay.

(Picture of an example)

If you only need to replay the actor transform, you can check the `AutoRecordTransform` when adding the ReplayActors for this ReplayActor type.

For other data such as animation or camera movement, there are events and functions inside the ReplayActor blueprints to script your ReplayActor logic.
