### The Fabric Of Cyber Space and Cyber Time!
... or
## Fabric of Cyber Space
... or
# Fabric

## What?
Fabric is a design of the software foundation for the Metaverse.

There currently exists a problem with the notion of the Metaverse, in that no software exists to support what it needs to become. No existing game engine, nor the web browser functions as it needs to. As all existing software foundations were conceived of with a screen-based mentality in their architecture and this is baked into everything to a root level. To properly support a spatial social space such as the Metaverse, you need to re-think the architecture of such a platform from the ground up on the presumption that different pieces of content will not be separate rectangles on a 2D screen, but will be separate 3D forms in a 3D space.

It may appear like this already exists, obviously we have so many 3D games, 3D game engines, and WebGL. The problem however is when we are talking about a rectangle on a 2D screen for a different application, at a technical level, we are also talking about a separate OS process and a separate chunk of OS virtualized memory for the application in that 2D rectangle. Different 2D applications are different processes on an OS, so if one 2D applications crashes, it does not take down all other 2D applications with it.

However, when we come to create 3D platforms, and we are talking one 3D form in a 3D space, be it an object, a world, or a player, that 3D form is not its own OS process, nor its own chunk of OS virtualized memory. Rather, currently, in all existing 3D platforms, every 3D form in a 3D space is sharing the same OS process and the same chunk of OS virtualized memory. If one 3D form in a 3D space in an existing 3D game engine has problem, it disrupts the performance of every other 3D form being displayed. If one 3D form in a 3D space crashes, it will crash the entire 3D space, because they are all the same process.

Fabric is a design to enable separate 3D forms in the same 3D space to be separate OS processes, with their own separate chunk of virtualized memory. So if one 3D form crashes, or is slow, in a 3D space, only that 3D form is negatively affected.

Technically this draws parallels to some implementations you can see in VR Compositor applications, like SteamVR Overlays. Fabric is a design to take it much farther, where separate processes don't just enable a different overlay on another application, but rather separate processes driving 3D content can co-exist in a 3D space interacting with each other through an IPC. So then complex 3D worlds with complex 3D interaction can be created where separate pieces of 3D content are truly technically independent, down to the OS level, akin to how 2D applications are on a screen.

## Why?
