# Setup Ubuntu 21.04 on ThinkPad X1 Extreme Gen. 3

This is a guide on how to setup your ubuntu install on the thinkpad x1e3. I'm mainly including how to setup the thinkfan service (as that one requires some research) and some general tips and tricks - everyone's machine is different, but it's good to start with some working hardware. 

## Thinkfan. 

Thinkfan lets you control the fans on your thinkpad, which I highly recomend you set up - the laptop will try to reach 30 degrees C before switching off the fans, which is unrealistic outside the polar circles. Go to the `setupThinkFan.md` file for more info.

## Tips

* All the video outputs are wired direclty to the Nvidia gpu - the gpu needs to be on to connect external displays... Nvidia Prime Render Offloading works pretty alright in driver 460. It will keep the gpu in a 1-2W state when you are not using it, and you can still use the igpu with reverse prime (sometimes laggy). You can run applications with the nvidia gpu by using the following flags:

```
env __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%
```

* I've included the original color profiles in this repository (for the OLED screen), in case you didn't back them up from windows. Their are the same across all machines of this spec.

* It's good to install powertop and set it up to auto optimize on startup - there's plenty of guides online on how to do that. It helps with monitoring misbehaving hardware, which this notebook _sometimes_ still does. 

* The display power draw does actually vary significantly (2-4W!) when displaying white vs black. Keep that in mind. The display usually draws somewhere around 5-10W on medium brightness - dark mode is your friend.

* If you're ever wondering why there's high power usage, try disabling wifi+bluetooth - you can see the power draw go down by 10W sometimes! I'm still investigating why this happens, it seems to be a problem in the Intel AX201 Wifi driver. If I come across a solution, I'll update this repo.
