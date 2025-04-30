There are extra steps to programming the CIDOO Nebula with VIA!!!

- Go to https://usevia.app/
- Go to "Settings"
- Toggle "Show Design Tab" and accept warning
- Go to "Design"
- Toggle "Use V2 definitions (deprecated)"
- Upload the CIDOO_Nebula_USB json file (in this directory)
- Switch to the "Configure" tab

Now you can edit to your heart's content!

There's also a 'cidoo_nebula.layout.json' in this folder, it's a backup of the layout from the last time I edited it. It shouldn't need to be uploaded or anything, unless the keyboard somehow loses it's config or something.

> [!NOTE]
> On Linux, by default, Chrome does not have permission to access the device and it will fail with a cryptic error
> - Go to `chrome://device-log/` to see the device path, in my case /dev/hidraw11
> - run `sudo chmod a+rw /dev/hidraw11` to grant permissions
> - reload page and re-connect to keyboard, should work now.
> Thanks to [this reddit comment](https://www.reddit.com/r/Keychron/comments/13nmnph/comment/kaas0rg/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) for the solution!
> (One other thing, mentioned in that comment, if you're running chrome from a Snap there are extra steps b/c it's sandboxed. But I run the native app so I didn't need that.)
