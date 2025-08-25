# A 'Metaverse Browser' or a 'Social Spatial Browser.'

## Sandboxing vs Walled Garden

### Sandboxing

What made the web browser significant was not solely its ability to display streamed content from anywhere on the web. 

People could technically send each other videos, images, articles or even applications before the browser. You could technically send a document with links to locations to download more content before the browser.

What uniquely made the browser significant was the ability to stream content from anywhere on the web into a sandboxed environment which you could reasonably trust would not harm your computer.

Second to TCP and the ability to send content in the first place. The sandboxed environment of the browser was the other catalyst for the growth of the web. It enabled people to create, distribute and view content without a worry of harm coming to their local machine.

This is an essential quality of a browser. Not only something that can stream content, but something that can stream content from anywhere, without restriction, without any single piece of content being able to disrupt or harm any other piece of content.

### Walled Gardens

The alternative to the sandboxed web browser was what came to be the Mobile App Store.

App Stores ensure security through a complicated submission and review process. Where an application is audited for malicious code and users are incentivized to only download applications from official locations.

This can enable a high degree of security and trust. This inherently requires a Walled Garden and closed ecosystem which explicitly mediates what content you can stream to the device.

This foregoes one of the quintessential qualities of the browser. That being, the ability to stream content from any source. However, the fact a walled garden does enable a high degree of trust was also a major catalyst of the Mobile App Stores.

### Primary Concern: People need to be able to trust the content.

This is the crux of what enables a mass of people to freely create and send content to others on the web. Leading to exponential numbers of people participating in a networked ecosystem of content compounding ever greater network effects.

## 3D vs 2D Sandboxing is a Novel Complication

### Running Untrusted Code Server-Side Only

Server side execution and offloading of compute to the cloud is a significant subject. For a wide range of applications this can be sufficient. However, this approach cannot work in every scenario. 

Any scenario where latency is a concern will struggle with this approach. As round tripping input to the server with updates coming back to the browser will be roughly 100ms on the best of internet connections. Which most people do not have the best internet connection possible. This would inhibit content which relies on real-time interactivity.

This model can also contribute to server costs. If you are trying to catalyze an ecosystem of content creators, server cost can be prohibitive to those who have little operating capital or are trying to make something that will generate little income. 

Only having server-side execution of content would make it more difficult for some creators to participate. It'd be important to have an ability for content to be statically delivered and fully executed in the browser itself.

However, server-side execution also opens many doors and should be available for where it makes sense.

The ideal route is a hybrid solution where certain logic could be selectively setup to run server-side, or in the browser, and you run the logic where it makes the most sense.

### Sandboxing Untrusted Code on the CPU

Sandboxing untrusted code on the CPU today is far simpler than it was in the 90's when the web first began. There are multiple solutions available.

#### JavaScript and WebAssembly Runtimes
Today we have multiple mature JavaScript runtimes that are designed around sandboxing of code. All of which also contain a sandboxed WebAssembly runtime. 

Despite not being the most optimal performance wise. These do offer a robust and mature security layer with adequate performance for many use cases. Relying on a Javascript Runtime also enables the platform to support Javacript which is important to attract developers.

Even if a newer, faster, approach to application sandboxing were used, it would still be beneficial to always include a Javascript and WebAssembly runtime for wider appeal. 

It is also the case that, currently, the only way to run JIT code on an Apple mobile device is through Apple's own JavascriptCore library. If the browser were to run on iOS or VisionOS it would need an avenue where all content could also execute on JavascriptCore with WebAssembly.

#### Sandboxing Untrusted Native Code

Modern OS's can now sandbox native applications effectively. Windows has AppContainers. Mac has Sandbox Profiles. Linux has seccomp. Albeit these too can have vulnerabilities or exploits, their security is actively maintained and patched. They could enable sandboxing of native application with full native performance.

Google's abandoned NaCl was an edit of GCC and Clang which enabled the compilers to build native executable with sandboxing mechanisms baked into the machine code itself. All of this code is open source and still available. It could theoretically be repurposed.
https://www.chromium.org/nativeclient/

There is also the newer, and still actively developed, Cosmopolitan LibC project. This enables a single executable with both x86_64 and ARM64 machine code within it to execute on any operating system. 
https://justine.lol/cosmopolitan/

Cosmopolitan LibC could theoretically be combined with Native OS sandboxing features to create a new kind of sandboxed native portable application runtime which could be used in a novel browser.

These are potential avenues of research for something better than a Javascript WebAssembly runtime. However there should always be a Javascript WebAssembly runtime available for wider appeal and portability. 

#### WebAssembly is slower than Native Code.

Google abandoned NaCl on the premise that WebAssembly would make it obsolete. It should be understood, despite promises of native performance from Web Assembly, that has never been achieved. Even the most minimal and optimal WebAssembly implementation is still 2.32x slower than native code. Commonly used WebAssembly runtimes in V8 or SpiderMonkey can be ~4x slower:
[Webassembly-benchmark-2023](https://00f.net/2023/01/04/webassembly-benchmark-2023/)

Due to philosophical differences in the WebAssembly committee concerning Dijkstra's article on goto statements from 1968. The changes needed to fix the performance of WebAssembly remain unresolved since 2016:
[WebAssembly Issue 796](https://github.com/WebAssembly/design/issues/796)
It is unlikely WebAssembly will ever be truly comparable to native code unless someone forked it and took it a direction the WebAssembly committee disapproved of.

### "Sandboxing" 3D Graphics.

#### This is the complication.

Sandboxing the CPU has straight forward solutions that are well-paved, or have clear paths to be usable.

GPU sandboxing is a completely different beast to which considerable novel invention is necessary.

Thus far no 3D engine, nor 3D platform, has been designed around the ability to stream any piece of 3D content, from any source, in a manner which could not disrupt any other piece of 3D content. 3D engines are designed on the premise that a knowledgeable team of 3D artists will feed it specially prepared and optimized 3D content.

The greatest struggle I see with the 3D graphical side of a metaverse browser has nothing to do with the fidelity of graphics. Rather, it has to do with rendering the graphics in a manner which can deal with any type of 3D content, from any source, of any quality, even malicious content, without disrupting any other 3D content. 

Knowledge and exploration around this is limited in the industry and can be particularly foreign to even seasoned AAA game developers, as it's a problem domain they have never had to deal with.

#### "Sandboxed" 2D Graphics are easier.

Current Web Browsers have WebGL and WebGPU where the browser can render 3D content in separate 2D framebuffers. This provides an easy solution to one piece of 3D content running slowly, or even freezing. If that one piece of content runs slow or freezes, then it will do so by itself, in its own rendering context window, and not disrupt the other 3D content.

All WebGL or WebGPU rendering contexts share the resource of the same GPU. So if one consumes too much GPU it can make the others slow down. However, different pieces of WebGL content being in separate rendering contexts enable them to render at different frame rates. One can slow down if necessary without disrupting others. 

If one piece of WebGL content crashes or has a serious issue, typically a browser can keep the other pieces of WebGL content running in their own rendering contexts or be able to recover them after a few seconds. Having such a disruption to 3D rendering in this scenario is not a huge issue because your screen will just sit frozen for a second or two, then come back.

However, when we come to 3D content in a shared 3D space, we lose those easy solutions to fault tolerance.

#### Complications of "Sandboxed" 3D content in a shared space.

If all content is rendering in a single rendering context, then all content has to update every frame, without disruption. Otherwise your entire camera and viewpoint would freeze.

If this is being experienced in an HMD, then it is critical there is minimal framerate disruption, or freezing, as that can cause nausea to the user, making the entire device uncomfortable to use. 

If all content is rendering in a single rendering context, then all content can update only as fast as the slowest piece of content.

To render large open areas filled with streamed 3D content from completely unknown sources, this is a huge complication. As it means everything has to render fast enough, in perfect sync, to not disrupt the ability of any other piece of 3D content to render.

There do exist a few solutions to this already out in the wild which we can learn from, but this is still largely a major R&D effort without a perfect solution.

### Shared render context with content filters.

This is the solution most tend to go towards because it makes the most sense with existing commercial game engines. If one piece of content runs too slow, or is too large, or is malicious, the user can selectively block it. 

#### Complications of content filters.

This sufficiently works in titles like VRChat, however, this has a handful of social implications in the platform. The major one being it disincentivizes open socializing and open sharing of content. Instead, creating a pressure for everyone to stay in small tight-knit circles of trust. Putting the burden of dealing with potentially poor running, or malicious content, on the end-user to selectively block ends up making public spaces notoriously bad. For periods of time in VRChat's running public social spaces were simply unusable because so many users would use malicious content to crash other users or make other users run excessively slow. Despite VRChat's popularity, this has been a major stain on its image and reputation, as it makes the platform extremely unforgiving and frustrating for new users or anyone who does not have a small tight-knit social circle to trust. I personally believe this has been one of the greatest barriers to the platforms growth.

VRChat somewhat mitigated the complication of this by forcing all users to use Easy Anti-Cheat which lets them exert stricter control on the executable. They also began to put stricter controls on the content which people upload, with all content having to go through their servers and validation process. This has made the platform more usable, but still not perfect. And it did so at the expense of it never being able to be open-source and always being reliant on verification processes on their proprietary servers.

This is also the solution VisionOS uses for its multi-app functionality. Apps use the RealityKit API to instruct a shared render context on what exactly to render but cannot directly control the shared render context. Then Apple deals with the potential of malicious or slow running content by requiring everything to go through their exhaustive App Store verification.

Apple also dealt with the issue of potentially malicious GPU shaders by making a restrictive subset of MaterialX shading nodes and blocking access to any real graphics API or shader languages.

A negative of this approach is that it requires some kind of centralized authority to scan content for issues before it can be delivered to any user. Or what is allowed to be content in the first place must be so restricted that it becomes prohibitive to the quality of content which can be made.

Despite the negatives, some functionality in this category is necessary in a metaverse browser for specific scenarios.

#### Controlled and limited shared render context as the baseline.

By default, you could have an API to control this shared render context with strict limitations such that it'd be difficult for a user to create disruptive content. This would be something in the direction of VisionOS RealityKit. Or it could be seen as a kind of 3D DOM. The purpose would be to enable enough 3D rendering functionality so it'd be possible to do something. With enough constraints and controls that you could reasonably stop malicious content or check if content is malicious in the browser itself before loading.

For example, this approach would require all 3D models to be in a known format. To which you could scan the 3D models for vertex count before loading and block content which is too heavy.

This approach could have strict constraints based on LOD and distance. Enforcing a constraint such as, at 10 meters away you can have up to 100,000 triangles. 100 meters away you can only have 10,000 triangles. If the user does not fill in the explicitly defined LOD level, then it will forego rendering.

Shaders would have to be predetermined and baked in. Perhaps you could allow a constrained subset of MaterialX nodes akin to VisionOS.

I see the purpose of this largely to be for basic, simple, content. For when someone wants to get a simple UI, or other simple piece of content into the world. It can also be a debug fallback layer for when someone is developing more complicated content.

This shared rendering content API could be built out first as the baseline. Unfortunately, something that ran entirely on such a constrained and limited shared rendering context API would struggle to provide engaging content and grow.

#### Controlled and limited creator APIs inhibit adoption.

Despite the Apple Vision Pro being incredibly advanced and having incredible brand power, and there being expectation of a less expensive consumer device will come in a few years with the promise of big payouts from the App Store. It has struggled to get quality content in the multi-app environment.

The RealityKit API to use the shared render context is so constrained in what typical 3D content developers expect that it has not attracted any serious efforts for developing quality Apps. Primarily, it has been small simple experiments from solo developers that failed to make the device and platform compelling thus far.

More professional and skilled creators don't want to invest time into what is perceived as a novel toolset unfamiliar to them with many constraints.

This is the major complication when you start talking about creating a controlled API to drive a shared rendering context, or a 3D DOM, or anything where you build the creation tools which users must become familiar with. As soon as you do this, you are no longer only competing with other potential metaverse-like platforms, but you are also now competing with content creation tools. You aren't only trying to convince users to build for your platform, you are also now trying to convince them to switch creation toolsets.

#### Building a new tool for content creators means you compete with other creator tools. That will fail.

Put simply, not even Apple could win that war. Nor has Meta been able to win that. This is why both of them still come to Unity to ensure Unity can export in some manner to their platforms. To compete as a creator tool would require years to get anywhere, and it is still unlikely.

Even with Unity being able to export to the VisionOS multi-environment, the constraints which that force Unity to work within have severely inhibited its adoption. Even that struggles to attract quality content creators even though a subset of Unity works, because it is still seen as a downgrade in capability.

To try and compete with all other metaverse-like browser platforms, and to also compete with all other 3D content creation tools is too much. That will ensure the entire effort fails. You cannot attempt to compete with both. This is a serious issue that needs serious strategy and foresight around. Getting content creators to adapt to any new tooling is incredibly challenging. This was the downfall of many metaverse platform attempts. No matter how good the platform is, this can easily be the failure point in adoption.

This is a significant component of VRChat's popularity because it allowed exports out of Unity with minimal changes to create content. This caused it to inherit millions of hobbyist developers already familiar with the tooling. The exception to this in recent history is Roblox, but they have spent years, and millions, on building out a competing tool before it managed to get anywhere. Along with the millions and years spent on the platform itself. 

#### There should still be a controlled and limited shared render context.

Despite the complications, there should be a new limited API to drive a shared render context. Whether that be something like a 3D DOM or other. That should still be built, and can be built first, but should be seen as only the default baseline for simple content, or a debug fallback.

#### A metaverse browser needs to inherit, not fight against, as many network effects that it possibly can.

Somehow a metaverse browser needs to inherit the network effects of all existing 3D tooling. Not compete with them. A metaverse browser has to be designed in such a way to be agnostic to all 3D tooling, and all 3D engines, yet still somehow enable content from any creator tool to be loaded into it.

For every single thing that the metaverse browser cannot inherit the networks effects of, that thing becomes competition and increases the chance of failure. The web is about inheriting and compounding established network effects.

### Multiple reprojected rendering contexts.

The alternative to one shared render context which renders everything is to have multiple render contexts which share their rendered framebuffers over an IPC and can be composited into the same shared 3D space.

This is essential what existing VR compositors such as SteamVR are. Both the Meta Quest and the VisionOS also have a compositor which underlays them. Currently, SteamVR is the only one that uses it to enable a commercial multi-app environment. Right now on Steam you can purchase VR apps known as SteamVR Overlays which can run on top of, and be composited on top of, any VR game. This enables an environment where multiple pieces of content can share the same 3D space. 

The merit of this approach is that it is agnostic to tooling and API. Any engine, or any tool, which can output the necessary set of framebuffer and position data necessary can have its content composited into a shared space. You can have every engine outputting something to the same shared space. 

There do exist standardized open APIs for how an application would output its data for use in a compositor. Namely, this is what OpenXR is for.

#### Multiple render contexts are complicated.

There is a list of Pros and Cons for this approach. To which the Pros are necessary to enable for creator tooling agnosticism. While the Cons can be overcome. 

#### Multiple render contexts can be slower than a single render context.

The first major complication. It can easily be slower to render many different pieces of 3D content in many separate processes and separate rendering contexts. 

The way you render lots of content the most performantly is to line up all data in one large piece of contiguous memory with a consistent predictable layout. Which simply can't be done if every piece of content has its own process and its own rendering context. 

If you had a 3D model you needed to render 100,000 times. A single render context could easily do that. Whereas trying to spawn 100,000 separate render contexts would struggle.

Multiple render contexts and multiple processes do not mean every single thing would be a separate process. Rather, these would be like App Volumes in VisionOS. Or the 3D version of an application window. Or the 3D version of a browser tab. Where within it, the single process would drive an entire game or an entire interactive experience. The limitation on how many of these you could have open would be similar to the limitation on how many browser tabs or applications someone could simultaneously have open. 

These would work in such a way that you could place down a volume in an area, which could be 10 feet by 10 feet, to 100 feet or more. It could be any size, but as you enter into this, it would be the 3D equivalent of moving your cursor into an application window, or a browser tab. That process would drive most all you interact with in that area. While other processes could still composite into the same shared space.

It could end up that avatar rendering is its own process. Perhaps you'd have several different avatar rendering processes to support varying avatar formats from different sources. With each process rendering a dozen, to more than a hundred avatars. 

Then you'd have a handful of environment application processes. These could be large 100 x 100 foot volumes placed right next to each other. To which only several could be seen at a time. If a volume is not visible, then it would not consume as much resource, similar to if you minimized a browser tab or place one application to be entirely hidden by another.

Theoretically, a high-quality experience could be managed with a few a dozen processes.

It'd also be ideal for the default, baseline, 3D DOM to be one of these processes.

#### Multiple render contexts can also be faster.

One of the benefits of the multiprocess and multi-render context approach is that it is innately multithreaded and can multi-thread content from different engines which may not individually be multithreaded.

What that means is, if you load 10 different pieces of heavy content into Unity. With each piece being unique, with its own 3D models, its own textures, its own shaders and its own unique logic. There is little that can be done to layout all that data contiguously for optimal execution. It will all end up running primarily on the single main thread.

Whereas if each of piece content was its own process, then each could fully run on a separate thread. It's in this way that heavy pieces of content which would be challenging, or impossible, to multi-thread in a single game engine process can become faster in separate processes, as then they run on separate threads.

This tends to be the nature of amateur 3D content. Where, even if those separate pieces of content were imported into the same 3D engine, they are made in such a way that being in the same process doesn't help. All it means is they must execute on the single main thread of that process.

This means, in the scenario of lots of 3D content of questionable quality from differing sources, there are scenarios where multiple processes and multiple render contexts can actually be faster.

#### Reprojecting separate render contexts is ideal for applicaitons with differing performance.

Since each application is its own process and its own implementation, they can all run at different frame rates without any issue.

VR compositors use a technique called reprojection which takes the last rendered frame of an application and continually reprojects it to appear in the correct location in the shared space. Which means, through a VR compositor, it's absolutely no issue if one application is rendering at 90 FPS, and another at 60 FPS, and another at 30 FPS.

If the FPS gets too low on any given application, this can introduce reprojection artifacts for that specific application. Depending on the type of content and implementation of the compositor, it can be tolerant to lower FPS. Content of the nature in VRChat which is largely static, with people sitting around talking, can still be usable even at 20 FPS. Generally, the more dynamic and faster the movement of something is, the more FPS it will need to appear correct in the same space. 

This enables setups where a process which is rendering only a static environment could run at a low FPS. While the process rendering avatars could render at an FPS multiples higher. This too creates a dynamic where it could be faster than a single process and single rendering context. Because you can selectively render parts of the world at different rates. You could theoretically start rendering parts of the world based on position and rotation delta rather than time delta. Giving you the ability to simply omit the overhead of a process dealing with some chunk of static environmental data if you aren't moving.

Whereas if everything was a single process and single render context, everything has to render at the same rate. No matter what. Even if 90% of the pixels on screen are the exact same. They all must render at 90 FPS for anything to be running at 90 FPS.

This offers an elegant alternative to LODs too. Where instead of requiring lower resolution meshes and textures to render something at a certain distance. You could lower the rendering rate of single pieces of content at a distance. Content at a far distance consuming a small 64x64 or 128x128 area of pixels could theoretically be rendered at several FPS. Only fast enough to keep some representation of those pixels present at a distance.

This all provides an elegant solution to slow running pieces of content, as it enables a piece of slow content to run slowly by itself. If something is too heavy, only its animation would not be smooth, or only it would have worse reprojection artifacts.

Currently, the ideal compositor to do this does not exist. The closest one is SteamVR. However, it still has constraints which would make this complicated. One of the major pieces of a metaverse browser is building the compositor which can do this.

#### Compositor would need to coordinate the FPS all the various rendering contexts.

To deal with multiple applications of differing performance optimally, the compositor would control the FPS of each application based on its distance and visibility, and the delta change of all movement related to it. This is already how OpenXR and OpenVR APIs work where applications wait on the compositor to signal to them when they should run a cycle. This enables the compositor to ensure the rendering load of multiple applications is staggered and balanced across multiple update cycles, and that each application only updates at the minimal rate necessary.

#### Separate render contexts can provide better fault tolerance.

In Windows or certain configurations of Linux, if a single render context crashes or stalls the GPU, that render context by itself can be shut down without disrupting other rendering contexts. This provides a mechanism by which you can truly sandbox the GPU commands and shaders. If some piece of content is malicious, with malicious shaders or operations, then it would only stall or crash that specific application, leaving the rest of the shared 3D space undisturbed.

Differing OSs are tolerant to this to varying degrees. Windows in its worst case scenario of fully crashing the GPU driver will freeze for a few seconds to shut down that render context before coming back and letting the other render contexts carry on.

Not all OSs, graphics cards and drivers are comparably fault-tolerant in this regard. For some users and devices we would still need some other mechanism to help mitigate malicious GPU content. However, keeping this door open is important, as it is the ideal way to deal with this, and as this kind of tolerance becomes more desired, drivers and OSs could be made to better work like that.

#### Enables inheriting of advanced functionality from other 3D engines.

Writing an advanced 3D engine is challenging. It would be sunk resources trying to simultaneously compete with Unity, Godot, Unreal and the plethora of simpler 3D APIs out there like RayLib or Three.js. If the metaverse browser can use content output from any of these other 3D tools, then it inherits all of their graphics fidelity.

This enables the default baseline 3D DOM to be basic. Leaving anything more advanced to those 3D engines.

### Technical Complications of Multiple Render Contexts.

This has purposely been high-level and not overly technical. Someone familiar should quickly have multiple technical questions come up about the notion of compositing multiple applications together in a shared space. This is not to be a technical spec, but to touch on some of the obvious concerns which come up.

#### Everything needs to output a depth map.

To depth composite applications together, each application must output a depth map. Then the compositor can accurately blend content from different applications. This is also needed for more accurate reprojection.

There has also been research done into other solutions. However, this has not been used in any commercial compositor:
[Shading Atlas Streaming](https://www.tugraz.at/institute/icg/research/team-steinberger/research-projects/sas)

We would probably end up with several techniques for an application to output the necessary buffers to be appropriately composited, and the application could specify which works best for it.

#### Transparency doesn't produce depth.

This is an area that will need a few solutions, and the content creator will have to choose which to make use of.

By default, a transparent surface would use the depth of the opaque geometry behind it. Which can be enough for many scenarios such as transparent grass on the ground, or a shallow pond of water.

The other option is the creator would need to ensure transparent objects which need depth somehow contribute to the depth. This could be done by a dummy object which does nothing but write depth, or by selectively allowing transparent shaders to write depth. 

For more complex scenarios the OpenXR API offers something called Composition Layers where you can specify objects to render to an entirely separate framebuffer. It could be made so an application would submit both an Opaque and a Transparent framebuffer and the compositor would combine them. So then the first layer of transparency could have its own accurate depth map.

There are motion analysis algorithms that can infer depth off stereo images or motion. The results of these are rough, but will also improve and are worth continued investigation. 

The Shading Atlas Streaming paper linked above offers an alternative solution by submitting individual triangle data to the compositor with a map of their shaded pixels. 

There are multiple solutions to this, and we would probably end up with a number of options creators could utilize. 

#### Inter-Application Interactions

The compositor can selectively forward input to a specific application. The OpenXR API needs no alteration to do this already.

However, inter-application interaction may require some novel invention.

OpenXR does have APIs for an application to have 'World Awareness' where mesh data and anchors from the world outside the app can be submitted to it for the application to be aware of and respond to. These APIs offer an already standardized solution to inter-application interaction.

This is something where the OpenXR spec would ideally be extended to allow potentially more comprehensive inter-app interactions.

This does not necessarily have to be performance inhibiting either. Shared memory between applications can offer a means for separate processes to share state with little to no overhead. The compositor would end up coordinating the access of shared memory between differing applications. Compositors already rely on shared memory for sending input to applications.

#### Shared Physics Simulation

Each application could have its own physics simulation. However, we'd also want a single, universal, physics simulation which applications could register objects to and receive callbacks from. So then multiple applications can interface through a shared physics simulation. 

OpenXR currently has no API concerning this, and it would need to be added. To which I'd note, you can add whatever you want to the OpenXR API and make use of it. Getting it ratified and standardized is what requires joining of the Khronos Working Group. 

#### Shared Lighting

This already has some existing APIs in OpenXR where the environment outside an application can submit a set of Spherical Harmonics to contribute to lighting in the application.

Continuing down this direction is probably the simplest and would provide sufficient quality. APIs could be added so that an application can also submit a set of its own Spherical Harmonics to the compositor, which the compositor would then selectively combine and make available to each application based on their specific location.

It is also possible to have a mode where applications themselves do not deal withing lighting at all. Instead submitting GBuffer like data of a Material ID and Normal Map to the compositor which could then apply a final lighting calculation which would match the other applications.

There are multiple solutions here, and it would be up to the creator to make use of one in their application.

This, of course, would not enable 'Physically Accurate Lighting' across the entire metaverse. However, that is not desirable. Perhaps certain, contained, applications can adhere to a physically based lighting model if it suits their need. But uniformly enforcing that across all content would require explicitly controlling all shaders, disallowing custom shaders, and it'd also inhibit creators from creating stylized visual styles. Physically accurate lighting needs to be opt in.

### The Hybrid Rendering Environment

Multiple Rendering Contexts are also an intermediary step to a Hybrid Rendering Environment.

There are multiple approaches to rendering currently on the horizon. You have traditional triangle-based rendering. Gaussian splatting, or triangle splatting techniques. You also entirely have AI neural engine driven algorithms. Much of the metaverse might also be ideally cloud rendered in a high-end 3D engine and then streamed to the end user.

If the browser can deal with, and composite together, the outputs of completely separate engines, then all of these rendering techniques could be selectively used in the same shared space.

A static background could come from an application using triangle splatting.

A high-quality virtual concert could be cloud rendered in Unreal Engine 5 and streamed to the user, with no local rendering taking place.

User avatars could be rendered in a Godot or Unity process locally with the traditional triangle pipeline to have the least latency possible.

User interfaces could be rendered in the baseline 3D DOM.

This notion will be increasingly important with mobile HMD devices which tend to have strict power and battery constraints that are diffucul to stay within.


