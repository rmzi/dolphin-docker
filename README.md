# Dolphin Emulator Docker Container
source: https://forums.dolphin-emu.org/Thread-how-to-make-dolphin-works-within-docker

## How to Run
* `docker build --network="host" -t dolphin-dato .`
* `docker run -it --net host --memory 2gb -e DISPLAY=unix$DISPLAY -v /media/storage0/ROM/dolphinconfigs:/root/.config/dolphin-emu -v /media/storage0/ROM/:/roms -v /dev/shm:/dev/shm -v /etc/machine-id:/etc/machine-id -v /run/user/$(id -u)/pulse:/run/user/$(id -u)/pulse -v /var/lib/dbus:/var/lib/dbus -v ~/.pulse:/root/.pulse --device /dev/input/event22 --device /dev/dri:/dev/dri --name dolphin-run dolphin-dato`

## TODO:
- Fix up README

## Author's Instructions:
```
docker run -it -> means "I want to do this command and directly interact with dolphin (means I wanna play Big Grin )
--net host -> same as above
--memory 2gb -> means limit the memory to 2GB, not "take them right now" (unlike a classic VM), it's not mandatory but bear in mind I run this on my server, I want to preserve QoS and stability, you could do this to the CPU too, I didn't...
-e DISPLAY=unix$DISPLAY -> this is to use my host display with docker (means graphical application)
 -v /media/storage0/ROM/dolphinconfigs:/root/.config/dolphin-emu -v /media/storage0/ROM/:/roms -> these are the two folder I want to export from my host into my container
-v /dev/shm:/dev/shm -v /etc/machine-id:/etc/machine-id -v /run/user/$(id -u)/pulse:/run/user/$(id -u)/pulse -v /var/lib/dbus:/var/lib/dbus -v ~/.pulse:/root/.pulse -> these parameters are there in order to use the pulse-audio from my user session (implies to select pulse audio in dolphin parameters), it's easier with ALSA but we are in 2017 men Big Grin
 --device /dev/input/event22 -> export my controller (PS4-DualShock 4 plugged by USB, rumble works, analog triggers are not analogs, need to investigate)
 --device /dev/dri:/dev/dri -> really important, this is for sharing the Direct Rendering Infrastructure (graphic hardware acceleration) to the container, works without but only CPU rendering.
--name dolphin-run -> name of the container (will be started with 'docker start dolphin-run' after)
dolphin-dato -> this is the name of the image (built above) I use
```
