### The Fabric Of Cyber Space and Cyber Time!
... or
## Fabric of Cyber Space
... or
# Fabric

## What? High-Level

Fabric is a low-level piece of tech intended to fill a gap in existing technology stacks that has emerged out of the evolving VR Metaverse use cases that is observable in platforms like VRChat, ChilloutVR, NeosVR, AltSpaceVR etc.

The problem which Fabric aims to solve is the enabling of users in a networked virtual space to be able to stream any content, from any source, of any quality, even malicious content, and then not have said content crash, nor disrupt, the end-user experience for anyone else who is also in that networked space. So for instance, if some user loaded in a poorly performing avatar into a virtual space, only that avatar would run at a low framerate, not effecting the framerate of any other content in the virtual space. If someone loaded in a shader on an avatar that was malicious and could crash users, only that avatar would crash, leaving the rest of virtual space and runtime unaffected.

Fabric is a solution to low-level security issues concerning user-generated-content in networked 3D platforms.

The purpose of Fabric is to implement security at the lowest level of wherever something can be exploited. A design principle of Fabric is to not mask, nor obfuscate, nor omit high-level functionality to create such security. Such as blocking custom shaders, or custom code. Rather, Fabric is an effort to create security by designing the low-level implementation in such a way where malicious content simply cannot be malicious.

Fabric does this by sandboxing everything related to both the CPU and GPU for individual, separate, pieces of 3D content in the same 3D context. This is partly similar to what a Web Browser does, but Fabric takes it further pertaining to 3D rendered content and the GPU as it deals with potentially slow, or malicious, 3D content loaded into a 3D context with other 3D content. Individual 3D items in a 3D space can be individually sandboxed both for security of their CPU calls and GPU calls, as well as their performance hit on the entire 3D context.

This does mean that different individual pieces of 3D content can render at different frame rates in the same 3D context. Fabric then relies on re-projection and interpolation techniques to fill in-between similar to what a VR compositor does, but Fabric has a special focus on enabling items running to run at a very low frame rate.

## What? Low Level

Any individual piece of 3D content can be a separate OS process. Not a "virtual process" in a game engine, but a real process with its own virtualized address space. 

A world can be its own process, but also different parts of a world can be their own process. Each avatar can be its own process. Games with a 3D world can be their own process, such as a board game on a table, or an arcade box. Even simple 3D items can be their own process.

This is very literally akin to how separate OS processes on your Desktop OS render their contents to separate 2D rectangles in the 2D space of your screen, however this is instead enables separate OS processes to render 3D content which gets composited together in a 3D space viewable through your screen, or a VR device.

The output of separate OS processes render their contents to chunk of shared GPU memory that is then composited together for display from a single parent application. This is very similar to how existing VR compositors like SteamVR work. The unique quality of Fabric is it is focused on enable very low framerate updates of individual pieces of 3D content, relying much more heavily on re-projection and interpolation techniques. It can also has a primary focus put on sandboxing the GPU in each separate OS process, so no single process can crash, nor disrupt the performance of, the rest of the 3D context.

Certain post-process lighting techniques would be applied, optionally, to final composited pieces of 3D content in the parent application, but internal to each process they may implement whatever lighting model they wish for their content. This does mean that the 3D space will not have a uniformly enforced lighting model. This is intended for reasons I will go into later.

Different processes will not be restrained to a single type of rendering paradigm. As long as the process can output the appropriate framebuffer data to the parent compositing application, then any rendering technique can be used. One process and piece of 3D content may be rendered via traditional triangle and texture pipelines of a GPU. Another process may be rendering via some novel neural rendering technique. Another process may be receiving an encoded stream of something rendered on the cloud. A major intent of this design and Fabric as a whole is to enable multi-rendering-paradigm 3D spaces.

These separate OS processes rely on an IPC mechanism in order to interact with each other. One process can publish a specific chunk of read-only shared memory for others to request access to, then subscribe to in order to read state about the 3D contents of that process and implement behaviour in response. While likewise any other process can do the same. 

Published chunks of shared memory would be done under a specific protocol depending on the type of interaction that read-only shared memory is supposed to enable. Any type of interaction between 3D content processes could have its own chunk of shared memory. These protocols for shared memory would be user defined, just as the content of the process itself would be user defined. This is to enable any new type of interaction between processes to evolve from the users themselves. 

All interaction between processes is to be done over these chunks of read-only shared memory and this security model of processes explicitly publishing read-only chunks of memory for other processes to see, and processes explicitly being given permission to read the state of that memory. Processes are internally responsible for dealing with any state publish by another process. 

For example, if in the process of a 3D world there was a collision detection system to prevent player's from running through walls. A player would publish its transform data for the world to subscribe to, the world would then publish back to that specific player the resulting movement of that player's transform, it would be the responsibility of the process running the player to then read the state from the world process and apply it appropriately. 

For the implementation of something like a traditional FPS shooter game, certain contracts on shared data and resulting behaviour could be checked by the world, so if a user join a world with a game and agreed to have their content interact with the world process over shared memory, then the world process can ensure the user is playing by the rules.

'Shared Memory' in this context is ultimately however an abstraction. If two processes are running on the same computer, the system will use shared memory by default, however if two processes are over a network they will transparently sync their content over a socket. The intent is to make a Network Transparent IPC that appears like, and acts like, shared memory but can automatically work over a network.

## What? Comparison To Other Tech

Fabric is a runtime that sits somewhere around the position of a VR Compositor, a windowing system (Wayland), OpenXR, an Operating System Shell or a Web Browser.

Fabric is neither any of those specific things, but rather it sits in a comparable position in the technology stack or should be seen in that category of technology.

Examples:
- Fabric is not itself a game engine, nor a Social VR application but someone could build an engine capable of supporting all things for a Social VR application on top of it, akin to how you could build a Social VR application on top of the SteamVR Compositor or Google Chrome.
- Fabric functions like a VR compositor in that it can composite the output of many processes together, but it is not itself a generic VR compositor, as it has baked into it many assumptions about the type of content that disallow it from being a general purpose VR compositor.
- Fabric can be a build target for VR experiences akin to OpenXR, but it is not entirely a replacement for OpenXR, but rather a layer on top that enforces very specific paradigms about how content is to be made to run on it. Someone creating a generalized standalone VR application or game would still want to use only OpenXR.
- Fabric has baked into it Operating-System-Shell-like functionality such as basic UI for system control and interaction, but is not itself an entire replacement for an Operating System Shell. It would only deal with 3D spatial functionality.
- Fabric is like a Web Browser in that it is built off many pieces of Web tech, and it's primary purpose is the full sandboxing of any content streamed into it, but it is not itself a replacement for a web browser.

## Why?

Much of the design of Fabric can seem like a complete abomination to anyone from AAA gaming background. I have often found the purpose of this project and the gap it aims to fill just completely baffles seasoned AAA devs as the design of it makes certain things far more involved than they would be in a typical game engine.

First and foremost it must be understood that Fabric is not intended to be a game engine in the traditional sense. Other games could be made to export to or be built in a way to run on Fabric, but Fabric is not itself trying to solve the problem of enabling someone to make a game.

It is far more appropriate to view Fabric more in the vein of a 3D operating system or a 3D web browser. An intent of Fabric is not just to run games, but to be able to run applications, real professional level applications in a networked 3D virtual space. A bigger first milestone for Fabric is not a VR game of any sort, but more so some sort of collaborative VR 3D modeling tool.

However, Fabric is something quite different from existing Operating Systems due to its inherent 3D nature. In a 2D operating rectangles floating on a screen that do not talk to each other at all is appropriate but in a 3D world separate pieces of 3D content are more expected to be able to affect each other. Where a traditional OS would be more concerned with the API/ABI of lower level kernel and hardware Fabric is more concerned with how processes can interop with each other and co-exist in a 3D space.

Fabric could at some point be a kind of OS Shell. This project https://github.com/evil0sheep/motorcar from 2015 I see to be of a similar intent of Fabric where Wayland itself was altered to allow application windows and 3D objects to be rendered in a 3D space. However, the additional unique quality of Fabric is that it also intends to be a kind of 3D Web Browser. To enable the streaming of sandboxed 3D content from other users, and allow multiple users to exist in a space simultaneously streaming their 3D content to each other.