## What is this? ##

This page contains rough concept and design notes and is subject to numerous edits. Minimally organized.

## What is the project about? ##

This project serves the following purposes:
  * learn Ruby language
  * gain programming experience (particularly game development)
  * develop a functional roguelike game engine
  * develop an experimental attribute/skill/experience/health/combat system
  * create a game featuring own unique setting (a short chapter)

# The RubyRogue Engine #

## Framework ##

RubyRogue is developed in [Ruby (1.9.1)](http://www.ruby-lang.org/) using [the Gosu library](http://www.libgosu.org).

## Project structure ##

### World structure ###

| **Bold** | high priority feature (must have) |
|:---------|:----------------------------------|
| Normal   | normal priority feature (can wait) |
| _Italic_ | low priority feature (nice to have, but not necessary) |

  * **World**
    * **Noise** (cached noise for procedural generation of regions and event locations)
    * _Normal map (for lighting etc., low priority feature)_
    * Time (date and time of the day)
    * _Sun (Vector of the sun, for lighting, temperature etc., Sun activity)_
    * _Moon (Vector of the twin moons for tides, brightness at night, Moons stage)_
    * **World Map** (for display purposes, not expected to be used much)
    * **Regions** (2D array of regions)

  * **Region**
    * **Elevation** (region's height value on world map)
    * **World location** ( X,Y on world map)
    * **Region Map** (for display purposes, likely the most used, also contains biome, temperature, flora, fauna, rainfall and such information)
    * _Normal Map (for lighting etc., low priority)_
    * **Entities** (which entities are present in the region, hash of arrays)
    * **Locations** (which unique locations are in the region, hash of locals)

  * **Local**
    * **Elevation** (location's height value on region map)
    * **Region location** ( X,Y on region map)
    * **Local Map** (for display purposes, an actual game view, also contains static terrain features)
    * _Normal Map (for lighting etc., low priority)_
    * **Entities** (hash of entities)
    * **Features** (hash of dynamic features such as chests, traps etc.)

# Game Concept Notes #

## Movement ##

### Pathfinding ###

The goal is to use pathfinding as least often as possible. The idea is to scatter pathfinding calls over time, plotting the path and stepping through the path until the last step is reached, then pathfinding is called again. Should be interruptible; for example chasing a mobile, if the mobile is closer to one of waypoints, further waypoints are removed. Pathfinding algorithm should take terrain features into account (difficulty, etc.).

### Movement cost ###

Take elevation changes into account. At first I considered using normal maps, however simply grabbing the height difference should be sufficient.

### Formation movement ###

Where groups are concerned, group members should copy the leader's path and follow it, thus the pathfinding call is called once only for the whole group. Alternatively, physics approach (rubber bands, pull members toward the leader). Groups could use formations as dictated by the leader, even flanking the enemy without having to run pathfinding.