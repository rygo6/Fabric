### The Fabric Of Cyber Space and Cyber Time!
... or
## Fabric of Cyber Space
... or
# Fabric

## What?

Fabric is a low-level piece of tech intended to fill a gap in existing technology stacks that has emerged out of the evolving VR Metaverse use cases that is observable in platforms like VRChat, ChilloutVR, NeosVR, AltSpaceVR etc.

The problem which Fabric aims to solve is the enabling of users in a networked virtual space to be able to stream any content, from any source, of any quality, even malicious content, and then not have said content crash, nor even disrupt, the end-user experience for anyone else who is also in that networked space. So for instance, if some user loaded in a poorly performing avatar into a virtual space, only that avatar would run at a low framerate, not effecting the framerate of any other content in the virtual space. If someone loaded in a shader on an avatar that was malicious and could crash users, only that avatar would crash, leaving the rest of virtual space and runtime unaffected.

Fabric is a solution to low-level security issues concerning user-generated-content in networked 3D platforms.

The purpose of Fabric is to implement security at the lowest level of wherever something can be exploited. Fabric does not mask, obfuscate nor omit high-level functionality to create such security. Such as blocking custom shaders, or custom code. Rather, it creates security by designing it in such a way where malicious content simply cannot be malicious, by appropriately sandbox everything related to the CPU and GPU. This is similar to what a Web Browser does, but Fabric takes it much further pertaining to 3D rendered content as it deals with potentially slow, or malicious, 3D content that could disrupt the entire 3D render context.

Fabric is a runtime that sits somewhere around the position of a VR Compositor, a windowing system (Wayland), OpenXR, an Operating System Shell or a Web Browser.

Fabric is neither any of those specific things, but rather it sits in a comparable position in the technology stack or should be seen in that category of technology.

Examples:
- Fabric is not itself a game engine, nor a Social VR application but someone could build an engine capable of supporting all things for a Social VR application on top of it, akin to how you could build a Social VR application on top of the SteamVR Compositor or Google Chrome.
- Fabric functions like a VR compositor in that it can composite the output of many processes together, but it is not itself a generic VR compositor, as it has baked into it many assumptions about the type of content that disallow it from being a general purpose VR compositor.
- Fabric can be a build target for VR experiences akin to OpenXR, but it is not entirely a replacement for OpenXR, but rather a layer on top that enforces very specific paradigms about how content is to be made to run on it. Someone creating a generalized standalone VR application or game would still want to use only OpenXR.
- Fabric has baked into it Operating-System-Shell-like functionality such as basic UI for system control and interaction, but is not itself an entire replacement for an Operating System Shell. It would only deal with 3D spatial functionality.
- Fabric is like a Web Browser in that it is built off many pieces of Web tech, and it's primary purpose is the full sandboxing of any content streamed into it, but it is not itself a replacement for a web browser.

## Why?
