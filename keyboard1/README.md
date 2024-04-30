# Keyboard 1

## Build Log

### Apr 30th 2024

Not actually changing anything right now, but I need to soon. Just grabbing some notes and links.

I need to switch to the true RJ11 connectors. They came in not long after I soldered the ones I have now, which I think are RJ12? They're slightly too wide. Still only 4 pin but the RJ11 cable has pieces of paper shoved in beside it to staqy aligned. Also, they don't fit down inside the case right so I had hot glued them to the outside and they have now come loose. I need to replace them before my backpack completely removes them for me.

I also really want to figure out the issue where I have to unplug and replug from the dock every time the computer is removed. It's really annoying. I found these posts online and thought I understood the issue, but the people on Discord said that I'm totally confused. so idk. I haven't yet had time to actually try any solutions.

Links:
https://github.com/qmk/qmk_firmware/issues/22477
https://github.com/qmk/qmk_firmware/issues/22778

Discord convo, copy paste not picture b/c pictures are annoying in markdown:

```
ofthedove — 04/23/2024 3:03 PM
Hi, I'm looking for help with a custom split keyboard I made.

The keyboard is usually plugged in through a powered USB hub, so the keyboard remains powered even when the computer isn't connected. This is a problem because when the computer plugs in to the hub, the keyboard doesn't connect to it. They keyboard has to be un-plugged and re-plugged to work.

I've done some searching and understand this is a common problem on split keyboards, as most split keyboards use USB detection to determine handedness. I tried to fix this by adding a handedness pin to my keyboard and firmware and explicitly set left as main, so that both halves should know at boot which side they are and whether they own the USB connection. However, the problem persists.

Is there a setting that will allow the keyboard to reconnect if the USB connection is lost, without power cycling? I think I saw something about a USB watchdog that would fully reset the board if it lost power, but that doesn't seem ideal or necessary to me. Aren't there other options?

Edit: Here's my info.json on GitHub for reference: info.json

Dasky — 04/23/2024 3:16 PM
Handedness has nothing to do with how the usb connection state is decided.
Which rp2040 based controller are you using?

ofthedove — 04/23/2024 4:36 PM
Interesting. That's the reason I've seen stated for why this is an issue for splits, that when the USB doesn't initialize both sides assume they're the secondary device. 

I'm using the Pi Pico.

Dasky — 04/23/2024 4:39 PM
when the USB doesn't initialize both sides assume they're the secondary device.
this part is correct but it has nothing to do with handedness
which pin are you using for power between the halves?
VSYS, VBUS, 3V3?
if you're using VSYS or 3V3 you should be able to enable VBUS detection

ofthedove — 04/23/2024 4:45 PM
VSYS

Will that make a difference? Since the hub powers the bus even when there's no host to talk to?

sigprof — 04/23/2024 4:45 PM
but only if you are using either the original Pi Pico, or some really good clone
with the original Pi Pico #define USB_VBUS_PIN GP24 should work

Dasky — 04/23/2024 4:46 PM
#define USB_VBUS_PIN GP24

sigprof — 04/23/2024 4:46 PM
but if you are using some lookalike with the Type C connector… there are lots of not-quite-compatible clones
and some of those clones don't have the VBUS detection circuit, or have other changes in the power circuits
```

Also, just throwing this in there, I want to add a compose key to the layout somewhere. That's the key that lets you put accents on words. On my laptop keyboard it's right alt but I don't have that anywhere on this keyboard.

### Feb 26th 2024

I started using the keyboard and realized that I can't capitalize Is anymore. Turns out, in the version of QMK I pulled a year ago the Mod Tap feature I use for space/shift and backspace/shift defaulted to a letter tap switching the key to it's hold behavior. That default got removed in the new version of QMK I just pulled in, which meant that the mod tap keys had to be held for the full time to access the hold behavior. Totally threw off my typing flow.

I figured this out by asking in the discord, and someone there pointed me to the documentation. Of course this documentation also assumed you were using the c and h files for config, not the info.json, but the info.json does have the right fields for it, and changing them worked. I made the change in the codespace, which is really nice to be able to tweak and download from anywhere. I had forgotten to consider that the codespace has it's own cache of the repo, so I got those back in sync, and also used it to create an `old-master` branch. I did that because I realized when I rebased `myStuff` all the commits were re-created, so the origianl commit dates were lost, and those will be useful in the future if I ever get around to back-filling this build log.

Oh, also, I picked the "Hold On Other Key Press" option rather than the "Permissive" option. I might want to switch to permissive later to make it easier to hit space without capitalizing by mistake.

Docs:
- Mod Tap Tap on Hold: https://docs.qmk.fm/#/tap_hold?id=tap-or-hold-decision-modes
- info.json: https://docs.qmk.fm/#/reference_info_json?id=firmware-configuration

### Feb 25th 2024

I needed to fix the matrix pin config issue before tomorrow, so I can have a working keyboard for work.

I removed the diode in the matrix and added resistors to both Pi Picos. I picked GP18 somewhat arbitrarily, and tied it to 3v3 on the left and ground on the right. I used a resistor instead of a jumper, so just in case that pin ever gets misconfigured as an output it won't be shorted. It's paranoid and unnecessary but not much harder so :shrug:.

I changed the handedness config in info.json from matrix to pin. There is in fact at least one example of this config in the repo, so that gave me a little extra confidence. I had to do some git magic to sort things out, I have no idea how I was able to build yesterday. I think I had merged upstream/master into my master, but I had meant to pull my changes out onto a branch so it will be easier to update in the future. So tonight I got that sorted, so that my master branch matches upstream/master, and my `myStuff` branch is rebased onto that.

I flashed the new uf2s and it seems to be working great! Woo hoo!

### Feb 24th 2024

So, this keyboard has one persistant problem that's been driving me nuts: it uses USB auto-detect to determine handedness. This seemed like a great idea when I first set it up, but the dock I have with my computer at work powers the keyboard even when there isn't a USB host available, which means the keyboard USB detect times out and every time I plug my laptop into the dock I have to unplug and reconnect my keyboard.

I decided I could fix that by using the unused matrix spot method. It seemed brilliant, my keyboard does have one unused spot in the matrix. What I failed to consider is that it's not the same spot on the two halves, because the matrix layout was made up as I went based on what seemed easiest to solder. The matrix pin config for handedness is also weird, it's a rarely used feature of a rarely used feature with very limited documentation. I had to pull upstream into my branch to get it to compile at all, which maybe explains why I didn't use this strategy during the initial build. It might have been entirely unsupported at that time.

Anyway, after getting it building and flashing the binaries, it... Didn't work at all. The right hand side had all the left hand keys, and the left side didn't work at all. I didn't bother trying to fight it, I just set it aside, it was already late at that point. And yes, I did check the diode direction. I know I did, because I had to desolder the diode to check because I forgot to do it before... Whoops.

