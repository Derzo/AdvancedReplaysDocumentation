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

Quickstart, ReplayComponent
----------
Add a ReplayComponent to an easily accessable blueprint class such as PlayerState or GameMode. This way you can access it and control the replay anywhere. You can have multiple different replays using different ReplayComponents, though usually 1 is enough.

![](http://i.imgur.com/Rtco5SK.gif)

Before you can start recording, you have to initialize the ReplayActors. The function for that is `AddReplayActors` (downer picture). You need to pass in all of the TrackedActor references, so you might need to store them into arrays first (upper picture). The `ReplayActorClass` inputs will be defined later.

![](http://i.imgur.com/8FuDBOS.png)

![](http://i.imgur.com/jtjJmqr.png)

It spawns ReplayActors using the inputs:

 Input | Description
------------ | -------------
`ReplayActors` | Define the added ReplayActors' properties
`ReplayActorClass` | What ReplayActor child blueprint class are you using
`TrackedActors` | References for every tracked actor in the scene that replays with the class above
`AutoRecordTransform` | Automatically record and play the actor transforms for these ReplayActors

To start recording your replay, call `Record`.

![](http://i.imgur.com/pjbJn0Q.png)

 Input | Description
------------ | -------------
`Time` | At what point in replay-time you start recording (Leave the first time recording at 0)
`KillReplayActors` | Automatically kills all of the ReplayActors assigned to this ReplayComponent
`WakeTrackedActors` | Automatically wakes all of the TrackedActors assigned to this ReplayComponent

When your recording is ready and you want to play it, call `Play`.

![](http://i.imgur.com/BBcLC6d.png)

 Input | Description
------------ | -------------
`Time` | At what point in replay-time you start playing
`Reversed` | Will the replay be played backwards
`KillTrackedActors` | Automatically kills all of the TrackedActors assigned to this ReplayComponent
`WakeReplayActors` | Automatically wakes all of the ReplayActors assigned to this ReplayComponent

That's it for the basics of using a ReplayComponent.

Quickstart, ReplayActors
---------------------------
First you need to think what actors you want to replay. The whole scene? Not a problem. Split them to different classes, such as a class for moving static mesh platforms and a class for the character.

You only need to do it for actors that will interact with something and be important. For example, you could let a background AI hamster live through the whole replay process, no-one would really notice if it's running on a hamster wheel when playing the game but sleeping in the replay.

To create a ReplayActor blueprint, right click, Create New Blueprint and select the class ReplayActor.

![](http://i.imgur.com/2lRXZE4.gif)

Open it up and you'll find nothingness. To add looks, add components. Most usual case would be to bring the static- and SkeletalMeshComponents from the TrackedActors into the ReplayActor. If you know their meshes & materials will be constant, you can set them up there, or if they can change between instances, use the function `CopyMeshAndMaterials` on BeginPlay.

(gif)

If you only need to replay the actor transform, you can check the `AutoRecordTransform` when adding the ReplayActors for this ReplayActor type.

(picture)

For custom data such as animation or camera movement, there are events and functions inside the ReplayActor blueprints to script your ReplayActor logic.

To record animation, there are 2 options. Either you replay the individual bone transforms, or you replay the animation blueprint values. The bone transform replaying is faster to do and more accurate for physics simulated bodies, while the animation blueprint method is way more efficient when it comes to performance and save sizes (if you are saving).

Create array(s) for the type of values you want to replay. Good tip is to use structures if you have many different types.

(gif)

Add event `RecordFrame`. This is called every time a new replay-frame is recorded (See the `RecFPS` setting in the ReplayComponent properties). From here, you can `AddItem` or `SetArrayElement` to the arrays. The event passes you the new frame index and the TrackedActor reference where you can get the data from.

(gif)

To play your replay, you can use the event `PlayFrame` or `Tick` along with the function `BlendFrames`.

If you need a smooth interpolated playback for something, use `Tick` and `BlendFrames`. Just find a linear interpolation node for the variable type and connect it all. Tick is only enabled when the replay is playing. I think.

(picture)

If not, use `PlayFrame`. It's called every time a new replay-frame is played (See the `RecFPS` setting in the ReplayComponent properties)

(picture)
