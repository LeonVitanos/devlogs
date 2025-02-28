# HEVR dev logs

- Repository: https://github.com/omega-gg/HEVR
- Azure CI: https://dev.azure.com/bunjee/HEVR/_build?definitionId=13&_a=summary

## Switching gears and fine tuning licensing - 6/25/2020

I've decided to switch gears recently, intially I thought I'd be focussing on rewriting the OpenVR
driver based on the alvr_server code. Unfortunately the debugging tools are a bit light on the current
build. Not only that, but also the fact that debugging often requires an Oculus Quest plugged in.

So I thought I'd focus on the HEVR client first, that will enable me to make it compatible with
the latest alvr_server while adding mocking, logging and debugging functionnality, mostly
displaying the video output directly on the desktop client.

The cool part about this client is that it will work on desktop and Android and act as an OpenVR
driver interface and a streaming client; depending on the context. These clients will also be able
to communicate via HTTP to propagate their current state and functionnalities.

Also, I'm not too bad at writing GUI(s) (see http://omega.gg/MotionBox) so I'll make it as
straightforward and powerful as possible.

So I'm essentially merging the Android Client, Web GUI and HTTP Server of the current ALVR web-ui
branch into a single binary. Now it's obviously a long road before everything works, but I hope
I'll have "mocking capabilities" which will be useful for me and ALVR contributors.

My goal is to be useful for the current ALVR users, possibly as a debugging or alternative client,
while opening new perspectives for the future.

Regarding licensing I've decided to update things a little bit for the HEVR client. Aside from
being GPLv3, I'm adding a private license option. This would enable me to release a closed source
ALVR+ client in the future to provide convenient functionalities like HEVR server discovery for
Cloud PC(s) (like ShadowPC).

Keep in mind that the HEVR client is a small part of HEVR, essentially the GUI. All the lower level
and crucial parts like libHEVR and HEVR-OpenVR that lets you build your own VR streaming
application from the ground up and very efficiently, will stay LGPLv3.

I want to stay true to my "omega mantra": innovative software that empowers people.
So I'm not going to close anything substantial and it would mostly be convenient stuff - if HEVR
gains traction. I'm inspired by what the Qt company is doing wiht Qt, maintaining LGPL licensing
while offering a private option. That fits my philosophical liberal view (not to be mixed up with
economical liberalism), which essentially wants to let people be free *and* responsible.

Don't hesitate to let me know if I'm missing something or if anything is unclear!

## Updating the project architecture - 6/08/2020

I've been iterating on the project architecture a bit and I thought I'd share the progress so far.

HEVR is essentially going to be divided in three main parts, libHEVR, HEVR-OpenVR and HEVR.
- libHEVR is going to be the basis for building a VR streaming application from scratch, both
client or driver.
- HEVR-OpenVR is going to be the SteamVR driver that depends on libHEVR classes with as few
specifics as possible.
The idea is to rely on generic classes as much as possible so we can port HEVR to different
scenarios in the future like OpenXR.
- HEVR is going to be a universal client (mostly for testing initally); it will also be able to do
mundane tasks in the desktop.
If it's ran from a desktop OS it will be able to install the OpenVR driver or launch SteamVR. It
will also be a viewer for debug.
If it's ran from Android on the Oculus Quest it will act as a streaming client like the current
ALVR-client (not a priority).

I'll keep HEVR-OpenVR entirelly compatible with the ALVR-GUI (web-ui) and ALVR-Client.
Ideally I would like HEVR client to be compatible with the current ALVR OpenVR driver
(alvr_server) thus enabling cross-testing.

The licensing will be as follow: LGPL for libHEVR, HEVR-OpenVR and GPL for HEVR client.
This architecture is inspired from the VLC architecture that puts most of its intelligence in
libVLC (LGPL) and builds its GPL client from it. That way I hope to fully promote Free Software
in the VR community while keeping the door open for proprietary implementations. Essentially
being liberal philosophically without "selling out" the GPL client (which wouldn't make much sense
anyway).

## Renaming ALVR2 to HEVR - 5/28/2020
So I wrote the following email to polygraphene:

    Greetings polygraphene,

    Hopefully everything is fine for you during these troubled times!

    First, let me thank you for the great contribution you gave to the community with ALVR.

    Along with a few contributors we have decided to keep improving on the original code from your GitHub repository. I've also started a rewrite of the alvr_server with native C++ and Qt (to make it work on Linux, among other things).

    I thought about calling it ALVR2. We want to keep the ALVR name as an hommage to the original project and keep the community together.

    Would you be okay with us naming that new repository ALVR2 or ALVRX (as one contributor suggested) ?

Since I didn't get an answer I decided to rename the ALVR2 repository to HEVR -> High Efficiency Virtual Reality.

That name is a geeky pun based on the HEVC (H265) encoder and an emphasis on the streaming nature of the project.
So I'll resume working on the project with that new name.

Even though I'm departing from the ALVR name I'd like to maintain the compatibility with
[JackD83](https://github.com/JackD83) ALVR, including the next GUI and Android client.

## ALVR2 project is up \o/ - 5/21/2020
I'm currently working on the new repository for ALVR2 (alvr_server v2).
I'm doing my best to keep the original project legacy and read about it.
My goal is to retain what makes it great while making it portable, easy to use, maintain and expand.

zarik5 has kindly shared wiki files with me regarding his project called ALVR-next that he started
writing with Rust. This should help me evaluate the best path to architecture the rewrite to handle
future improvements.

Repository: https://github.com/omega-gg/ALVR2

The initial project is now up an running, displaying a beautiful "Welcome to ALVR2" from the console.
CI should be functionnal on Windows (gcc / msvc) / Linux / macOS.

My rewrite is going to use Qt because multiplatform without a good framework is hell.
An alternative would be Boost but I have more experience with Qt and I think I like it better.
It's also going to be based on my application kit called [Sky](http://omega.gg/Sky) that I designed
for MotionBox. License wise, I decided to go from GPLv3 to LGPLv3 to make it suitable for the ALVR2
developpement. This should give me a headstart for various mundane tasks like handling configuration
stuff. Being a single developer on this, it's a good thing to avoid reinventing the wheel.
