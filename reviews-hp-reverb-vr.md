# HP Reverb G2 VR Headset Review

I bought the HP Reverb G2 VR headset for half price in a Black Friday sale. I've been pretty happy with it over the last 6 months.

I ended up with the Reverb G2 instead of the Index because you can't buy an Index without buying controllers or lighthouses. But I had already bought the Index controllers during the video card shortage &mdash; no point in a high-resolution headset on a card that can't drive it, right? Fortunately, I found that people have made the Reverb G2 work with Index controllers. More on that later.

## Hardware

The headset is a high-resolution (2160x2160 per eye) panel together with headgear based on the Index, including the attached headphones. That's the positive side; the negative is that everything else is pretty average: the panel is a LCD with a 90 Hz refresh rate, the lenses are not amazing and the field of view is 98 degrees. Fortunately, resolution is what I missed in my previous headset, the HTC Vive: The Reverb G2 is a little more than 4K, whereas the Vive is a little more than 1080p. (The Index is a little more than 1440p, also known as 2.5K.) 1080p in a VR headset feels like 600p on a TV, and 4K feels a lot like 2.5K.

The headset and controllers use visible-light tracking. It's actually all headset: it tracks its own position by looking at its surroundings, and it tracks the controller positions by looking for a pattern of LEDs on the controller rings. Headset tracking is solid as long as you have enough light, but it is unreliable when I have to keep lights low, like after my kid is in bed. However, as long as the light level doesn't change, the software will inform you ahead of time whether you have enough light. The controller tracking is decent, but not as good as my Index controllers or Vive controllers -- just a *liiittle* wobbly, visible when the controllers act as laser pointers, say. Overall, it's good enough, but not the top-of-the-line.

The headgear: that is, the mask, strap and face gasket, all fit great and are comfortable. I've played for 2 hours at one go and the only discomfort I had was eye strain. One particular feature I like is the hardware IPD adjustment, especially given the small sweet spot of the lenses. However, it still doesn't *quite* cover my IPD. I have very close-set eyes for an adult male, but they're within the average range for females and children. I'm not sure what the problem is here but no headset has solved it yet, but until it's solved, VR won't be accessible to half of the population.

The speakers, which copy the Index design, feel like a magic trick. They're attached to the headgear, so they don't clamp to your head, and they're trivial to put on. Yet they sound great and people around can't hear them unless there's a loud, sustained noise in-game. And if you still want your own headphones, you can easily unscrew them.  However, the contacts are not solid -- I had to clean them after one speaker stopped transmitting sound. Points off for that, but points back for the speaker being easy to unscrew and clean.

The controllers are poor. They copy the proven design of Facebook's Quest controllers, but they are *extremely* cheapy. The rumble is a weak, high-pitched buzz that would have better been left out. The buttons are clicky  and accurate but they feel **bad**: hollow and light.  Fortunately you can use other controllers with the right tracking hardware and SteamVR plugin. More details below.

## Software

It's a mess. You start by plugging in the headset, which launches Windows Mixed Reality (WMR), which is, as far as I know, abandoned except for fixes and Steam compatibility updates. This works smoothly, but doesn't have anything except yet another pointless skeuomorphic home-style launcher. To stop, unplug the headset: simple but slightly surprising.

To actually *use* the headset for playing games, you need to launch SteamVR and install a small plugin. This works as well as SteamVR ever does, but with extra bugs. Specifically, if games request too much VRAM&mdash;and for this high-resolution headset, they will&mdash;the WMR driver has had problems with memory leaks followed by crashing throughout 2022. (Based on reading release notes.)

I recently upgraded to a 12 GB Nvidia card which helps a lot, but several games crashed a lot when running at full resolution on an 8 GB card. You can reduce the resolution, and because of the way scaling and pixel fill works on a high-resolution panel, it still looks better than a low-resolution panel. But it's disappointing in a PC-gaming kind of way, in which you have to turn down a barely-noticeable setting because your computer isn't good enough.

Finally, if you want to use index controllers (see next section), there's additional setup each time.
All in all, the experience is better than my HTC Vive in 2016. Specifically, there are fewer steps to get started, which poses less of a barrier to playing each time. (Also it helps that the headset is much more comfortable, which poses less of an unconscious barrier to being subtly tortured.)

### How to use Index Controllers

(Based on [instructions from UploadVR](https://www.uploadvr.com/how-to-use-hp-reverb-g2-with-valve-index-controllers/))

1. Plug in the Reverb headset and HP controllers and start SteamVR.
2. Install [OpenVR Space Calibrator](https://github.com/pushrax/OpenVR-SpaceCalibrator) in SteamVR. 
3. Close SteamVR, plug in and connect USB for the Vive headset (but not video).
4. Start SteamVR, turn on lighthouses and index controllers.
5. In the SteamVR menu, start the plugin and sync the controllers.
6. Turn off the HP controllers.

After setup, do this:

1. Plug in the Vive headset and lighthouses, but don't connect the video or USB.
2. Plug in the Reverb headset (but not the HP controllers, you don't need them anymore).
3. Connect the Vive USB (but not video).
4. Start SteamVR.
5. Turn on the Index controllers. 

Note that if the headset can't find the bounds, or loses positional tracking before you start SteamVR, then you'll have to resync the controllers. When you do, remember to sync the Index controller with the HP *controller*. The Space Calibrator will also offer to sync to the HP headset, but that's a lot harder than holding two controllers next two each other and waving them around.

## Games

Pretty anaemic. I stopped paying attention for 3 years and there have been a trickle of popular new games in that time. Looking at the top games on Steam, they're largely the same as 3-4 years ago. 

Facebook is in the process of pivoting away from VR to AI. VR has real advantages for some things, but I think it's going to be dormant for a while until somebody makes good AR hardware, at which point VR will come along for the ride in domains where it makes sense.

Over the last 6 months, I've played 3 full games: COMPOUND, Psychonauts and Half-Life Alyx; a few VR ports like Skyrim, plus nice updates for the same old games I've had since 2016: Subnautica, Elite Dangerous and Obduction. Oh, and 2-3 new games that are basically tech demos or low-budget prototypes: Boneworks, Into the Radius. Interesting, but not actually fun to play.

The easiest way to summarise all this is: Skyrim VR is the best "new" game I played. It's the same game, but a new experience in VR. VR is a real advance in videogames. There are two genuinely new, genuinely good interactions: aiming and grabbing. That plus head-tracking and stereography makes it about as big as the switch to from 2D to 3D games. The problem is that current 3D games are highly polished and VR is expensive, complex and uncomfortable, so there's not enough reason to switch.

## Addendum: Comparison to Apple Vision

Can you do work with the Reverb?

No.

The software isn't great, but more importantly, the lenses are too fuzzy outside the central sweet spot. This isn't a big problem in a game, since the more action there is, the more you tend to look straight ahead. I noticed it much more when scanning text. Finally, there's a cord which unavoidably tethers you to the outside world when you move around.

The Reverb is quite comfortable, and&mdash;who knows&mdash;could be as comfortable as the Vision. But, in my opinion, the number 1 obstacle to working in VR all day is having to wear something that can never be more comfortable than a ski mask. I do think that AR will be the thing that breaks into the mainstream, and maybe an approximation with a VR headset will be enough to get started.

## Summary

Overall, I feel like this is a good end-of-2nd-gen headset if you can find it for cheap enough. It nails some aspects of the experience, but has real shortcomings in many others. It's nothing like a 3rd generation product that manages to address all aspects of the experience adequately for a decent price. And HP seems to agree with me, since I feel like the company is treating this like the last headset they'll make for a while; the Black Friday sale felt like an attempt to clear out inventory. VR gaming is likely headed for a dormant period, so if you're an enthusiast, the Reverb, the Index or the PSVR2 are your best options for the 2nd generation.

I've been pretty critical, but I like this headset. If you can find it for less than $600, and have a way to replace the ridiculous controllers, I'd recommend it.

## Addendum: 2025 Postscript

This was a bad recommendation. After abandoning Windows Mixed Reality software, Microsoft [went on to **disable** all Windows Mixed Reality headsets in a Windows Update](https://www.uploadvr.com/windows-11-24h2-kills-windows-mr-support/) in late 2024. You should to gauge Microsoft's interest in their closed-source software closely before you decide how much to rely on it.