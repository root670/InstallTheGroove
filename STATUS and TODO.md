### OS

#### Status

The operating system is Fedora 21. The choice of this OS is that it is a modern OS, has wide hardware support, and I have the most experience with RPM-based distros.

The "Server" edition is used solely because it installs the smallest amount of packages. We want only the essentials, and nothing more. The user is always able to install more via yum.

Fluxbox is chosen as the default window manager due to how lightweight it is, while still giving the user basic functions. A right-click of the desktop will give you access to a few basic apps. Adding menu items has been scripted into the kickstart, launching Stepmania 5, OpenITG, and other utilities.

Pulseaudio is specifically removed from this distribution because it hampers the games. It introduces audio lag, and OpenITG will flat out not output audio with it installed. The audio is piped directly from the hardware out to ALSA, right to your speakers.

#### TODO

Create a different default user, ~~and make it auto-login.~~

Autorun the game upon logging in.
- This will be controlled by a bash script. If both games are installed, an accompanying script will provided to set the default.

Utilities script
- This will allow the user to do functions like recache, change themes, preferences, other lines inside INI files, via a menu-driven script. This will save the user pains of opening the files in external editors to make common adjustments.

Test on real hardware. I have no hardware to test on. I am currently looking for volunteers to help test. I have no idea what is required for the kernel to recognize the hardware, and what is required for the games to see the exposed hardware.

Auto-detect monitor and set the X11 config as such.

Auto-detect the hardware in use (JPAC, PIUIO, ITGIO, Minimaid) and adjust JWA to compensate if needed.

### Stepmania 5

#### Status

Stepmania 5 compiles and runs without issue on Fedora 21. I have built it into a binary RPM, and provided my spec file to go with it. Stepmania5 works best without pulseaudio, so InstallTheGroove doesn't use it. Support is still compiled in (for users who don't wish to use InstallTheGroove) so only the pulseaudio *libraries* are installed.

Stepmania 5.0.6 will build cleanly, but building the RPM with the included spec file makes requires some detail: the command to toss *rpmbuild* is `QA_RPATHS=$[ 0x0002|0x0004 ] rpmbuild -ba stepmania-5.0.6.spec` due to the way ffmpeg is compiled into the game statically. I wish there were an easy way around this but there isn't. Future builds of stepmania will use *cmake* instead, and I'm actively working with the dev team to ensure that the RPMs can be built.

#### TODO
~~Build new RPM based off of latest release. Mine is probably a month out of date. With the spec file, it makes life easy.~~

~~Work with Wolfman2000 to ensure *cmake* knows what to do on a `make install`~~

### OpenITG

#### Status
I took a binary build of OpenITG R2 and dropped it on my 64-bit Fedora21 VM, and kept installing the required 32-bit libraries until OpenITG would successfully run. As such, it's not really a F21-based build, but it still runs.

#### TODO
~~I have to track back through my yum install history to get all the 32-bit libraries I installed to get oITG running. From there, I can build a spec file and get an RPM of this sucker out there.~~

Create a Simply Love RPM for OpenITG, much like I did for Stepmania 5.

### Misc TODO

Spend the 5 minutes putting the RPMs in a new directory and running 'createrepo' for an actual yum repo
- Eventually this repo will expand to host all sorts of user-created content. This will include (but not be limited to) themes, arrow skins, and song packs. My server has a generous amount of space and bandwidth.
- I'll have a place where users can upload their files and specify the type, and the server will automatically pick it up, convert the .zip into an RPM, and drop it in the repo automatically. *This will be the shit.*

### Bucket List

Online score tracking. I really want this to be a thing. Submitting the score XMLs to a server and parsing it into a database shouldn't be anything crazy. We can use something like incrond to watch the directory for changed files (new score) and send the changes to the server via a script in the background.
