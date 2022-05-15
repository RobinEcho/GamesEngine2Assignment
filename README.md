# GamesEngine2-Assignment
# Description
* Student Name: Junjie Peng
* Student Number: D18124895
* Program Code: TU856/4
* A brand new space battle scene in unity. The timeline manager controlled the whole scene, which is easy to manage all game objects. All ships are controlled by the behavior tree. The scene runs automatically.


# Video Demo:
[Google Drive Link](https://drive.google.com/file/d/1B9uwKoDVdS3ou5rBXcSCazq8k7VZeXHz/view?usp=sharing)


# How to use
* Open the project through unity and simply run it
* The project runs automatically and will be closed once all scenes played
* Press escape to close it at any time


# Story Board
![Storyboard](/StoryBoard/001.png)
* Razer Ship warping
![Storyboard](/StoryBoard/002.png)
* Space Station First Person view
![Storyboard](/StoryBoard/003.png)
* Wingman ship inbound and being formation with Razer
![Storyboard](/StoryBoard/004.png)
* Enemy Mother ship arrived
![Storyboard](/StoryBoard/005.png)
* Enemy Fighter ship spawned and move close to Razer
![Storyboard](/StoryBoard/006.png)
* Dogfight between alliance and enemy
![Storyboard](/StoryBoard/007.png)
* Reinforcements inbound
![Storyboard](/StoryBoard/008.png)
* Alliance mother ship inbound
![Storyboard](/StoryBoard/009.png)
* Super cannon charging
![Storyboard](/StoryBoard/010.png)
* Nuke flying towards to enemy mother ship
![Storyboard](/StoryBoard/011.png)
* Enemy Mother ship escape from the battlefield


# Events Summary:
* Razer ship arrived Tinniuq by warping, camera shaking
* Razer ship slow down, camera set as look around space station
* Friendly sparrow spawned and follow Razer formation as wingman ship, camera switch to third person view and show friendly sparrow
* Blue protal appear
* Alien Mother ship cross through the gate and stop nearby the space station
* Alien fighter spawned and camera moved for displaying the formation of enemy ships
* Camera moves back to Razer ship, switch to be the back of Razer
* Alliance and Alien dog fight
* Razer calling for support
* Another blue portal opened, friendly ships jump in. Camera moves nearby the new gate
* Camera back to Razer, eliminate all enemy with help of support
* Alliance mother ship jump in, camera going to look at it
* Mother ship charge cannon, charge Effect start
* Camera moves towards to cannon
* Nuke lunched and fly towards to enemy mother ship
* Enemy mother ship hit and escape from battlefiled
* Camera move back to space station
* Enemy mother ship disappear
* End

# List of classes/assets in the project

| Class/asset | Source |
|-----------|-----------|
| CameraManager.cs | Self written |
| GameManager.cs | Self written |
| SpawnManager.cs | Self written |
| Lunch.cs | Self written |
| PartsRotation.cs | Self written |
| Radar.cs | Self written |
| PathEditor.cs | Self written |
| BaseBehaviour.cs | Self written, adapted throuhg coruse code |
| PathFollowBehaviour.cs | Self written & inherit from BaseBehaviour.cs |
| AddtionalBehaviour.cs | Self written & inherit from BaseBehaviour.cs |
| BattleBehaviour.cs | Self written & inherit from BaseBehaviour.cs |
| ObstacleAvoidBehaviour.cs | Self written & inherit from BaseBehaviour.cs & adapt from course but totally rewrite|
| PatrolBehaviour.cs | Self written & inherit from BaseBehaviour.cs |
| SpawnFriendlyBehaviour.cs | Self written & inherit from BaseBehaviour.cs |
| Shipstatus.cs | Self written |


# Key functions

## Cameras
* The cameras in scenes are multiple cinemachine also known as virtual cameras. By setting many cameras with the same priority, the last camera woked up will overwrite any other camera 
* The cinemachine holds two main parameters, follow will update its position, and look at will set the camera to watch which objects based on follow object.
* By applying Perlin noise on camera, a warping effect can be achieved like the start of the scene, the screen shaking to simulate the high-speed warp.

## AI
* Instead of the Finite state machine that is hard to maintain and add a new state if there are too many conditions. The behavior tree was used to manage AI
* Behaviour tree holds a root that runs all programs like the entry state in the finite state machine. each node or leaves with different conditions can lead to new behavior
* Every ship should have a basic boid and behavior which can be inherited by other additional behavior. with panda behavior extension. a public node can be created and edited by text file simply
* Conditions among all behavior are all around ray and collider. for obstacle avoidance, the script will lunch many rays based on parameters of angle and vision, once the ray hit other colliders, it will add force in the opposite direction to prevent hitting the obstacle
* Path follow will detect the path holder and read its child objects which are nodes or checkpoints. apply force towards each checkpoint and once it reaches one of them, access the next waypoint until it reaches the last checkpoint.
* Patrol behavior will apply to a central object, with patrol radius and reach distance, the object will move to a random point around the center object, because the movement of all objects is based on the rigid body or physical simulator, it is hard to stop exactly the point, so apply a reach distance so that the object has no need to arrive exactly where the points are.
* Battle behavior will check the collider firstly, then access its tag, if the tag was contained in its enemy tag list, it will transfer this collider as target and apply a force towards it. At the same time, the radar script which is not a behavior and applied at the launcher object of ships will accept the target transform and lunch prefabs based on conditions like range, cooldown time between each fire
* The priority of all behaviors are based on the position of behavior tree, the parameters of the pandas behavior tree can be used to whether all trees going to be run at the same time or not or how it stopped based on each task state, for example, the race parameter runs all child tree at the same time and will be stopped at the one return false but all task before it will keep running again and again

## Visual Effects
* Skybox: A skybox was applied to simulate the space visuals
* Particle system: particle system is used to create random dots that move around the whole scene, and the fog was also created by the particle system. All particles are used to fill up the space to make the static background looks dynamic
* Visual Effect Graph: The visual effect graph was used to handle the complex visual effect like the portal or charge effect. Not like the particle system, VFX provide a visual map editor to handle multiple effects in one object and easily manage how each effect works at the same time.
* Trail renders: to make all laser and other projectiles visible in dark space, every projectile applied trail by trail render. it created a long trail at every position signed and fade out under the control of its lifetime.

## TimeLine 
* Timeline is a unity package that controls all objects signed to it. By creating an action task, it will wake the signed object up at a specif time
* Virtual cameras in the scene are applied at multiple game objects like the previous part said. the timeline will wake each camera at the correct timing so the camera can be edited easily
* By using the timeline, many functions can be placed inside the start function of the script, once it is woked up through the timeline, it will only work once, this prevents potential problems not only for task management but also for ai actions
* Timeline can also manage audio like a game object, with the timeline. It is much easier to manage each task as a sequence

## Audios
* Most audios are managed by timeline, but there is some sound effect like laser sound effect was bound on the projectile itself, once it was instantiated. the audio will be played automatically 


# Resources

## Audio
* All audios of people was created by me throuhg text-to-speech software [Replica Studios](https://replicastudios.com/?gclid=Cj0KCQjwyYKUBhDJARIsAMj9lkEeuS-gHw_VHUjfqq8aacvsALONXJ7vPIEg_1yJCKJdEz0pA3JtiC4aAi7VEALw_wcB)

* Background music was purchased through [Fanatical](https://www.fanatical.com/en/)

* Sound effect like explosion, laser lunch are from open-source website [opengameart](https://opengameart.org/)

## Models
Modules are from asset store or created in blender like debris.

Space ships Model from [Ebal Studios](https://assetstore.unity.com/publishers/24304):
* Hi-Rez Spaceships Creator Free Sample
* Star Sparrow Modular Spaceship

SkyBox Model from [Sean Duffy](https://assetstore.unity.com/publishers/4239):
* Deep Space Skybox Pack

