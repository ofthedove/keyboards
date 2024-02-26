# Keyboard 1

## Build Log

### Feb 26th 2024

I started using the keyboard and realized that I can't capitalize Is anymore. Turns out, in the version of QMK I pulled a year ago the Mod Tap feature I use for space/shift and backspace/shift defaulted to a letter tap switching the key to it's hold behavior. That default got removed in the new version of QMK I just pulled in, which meant that the mod tap keys had to be held for the full time to access the hold behavior. Totally threw off my typing flow.

I figured this out by asking in the discord, and someone there pointed me to the documentation. Of course this documentation also assumed you were using the c and h files for config, not the info.json, but the info.json does have the right fields for it, and changing them worked. I made the change in the codespace, which is really nice to be able to tweak and download from anywhere. I had forgotten to consider that the codespace has it's own cache of the repo, so I got those back in sync, and also used it to create an `old-master` branch. I did that because I realized when I rebased `myStuff` all the commits were re-created, so the origianl commit dates were lost, and those will be useful in the future if I ever get around to back-filling this build log.

Oh, also, I picked the "Hold On Other Key Press" option rather than the "Permissive" option. I might want to switch to permissive later to make it easier to hit space without capitalizing by mistake.

Docs:
Mod Tap Tap on Hold: https://docs.qmk.fm/#/tap_hold?id=tap-or-hold-decision-modes
info.json: https://docs.qmk.fm/#/reference_info_json?id=firmware-configuration

### Feb 25th 2024

I needed to fix the matrix pin config issue before tomorrow, so I can have a working keyboard for work.

I removed the diode in the matrix and added resistors to both Pi Picos. I picked GP18 somewhat arbitrarily, and tied it to 3v3 on the left and ground on the right. I used a resistor instead of a jumper, so just in case that pin ever gets misconfigured as an output it won't be shorted. It's paranoid and unnecessary but not much harder so :shrug:.

I changed the handedness config in info.json from matrix to pin. There is in fact at least one example of this config in the repo, so that gave me a little extra confidence. I had to do some git magic to sort things out, I have no idea how I was able to build yesterday. I think I had merged upstream/master into my master, but I had meant to pull my changes out onto a branch so it will be easier to update in the future. So tonight I got that sorted, so that my master branch matches upstream/master, and my `myStuff` branch is rebased onto that.

I flashed the new uf2s and it seems to be working great! Woo hoo!

### Feb 24th 2024

So, this keyboard has one persistant problem that's been driving me nuts: it uses USB auto-detect to determine handedness. This seemed like a great idea when I first set it up, but the dock I have with my computer at work powers the keyboard even when there isn't a USB host available, which means the keyboard USB detect times out and every time I plug my laptop into the dock I have to unplug and reconnect my keyboard.

I decided I could fix that by using the unused matrix spot method. It seemed brilliant, my keyboard does have one unused spot in the matrix. What I failed to consider is that it's not the same spot on the two halves, because the matrix layout was made up as I went based on what seemed easiest to solder. The matrix pin config for handedness is also weird, it's a rarely used feature of a rarely used feature with very limited documentation. I had to pull upstream into my branch to get it to compile at all, which maybe explains why I didn't use this strategy during the initial build. It might have been entirely unsupported at that time.

Anyway, after getting it building and flashing the binaries, it... Didn't work at all. The right hand side had all the left hand keys, and the left side didn't work at all. I didn't bother trying to fight it, I just set it aside, it was already late at that point. And yes, I did check the diode direction. I know I did, because I had to desolder the diode to check because I forgot to do it before... Whoops.

