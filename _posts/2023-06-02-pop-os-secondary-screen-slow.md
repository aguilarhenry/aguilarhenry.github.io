---
layout: post
title:  "Fix for slow secondary screen in PopOS"
date:   2023-06-02 15:42:52 -0600
---

If your second monitor in linux is set as primary and your applications on that monitor are very slow, this is a fix.

So it's an X11 error. It happens to people with very varied configurations (gpus, distros, etc).

There is a fix by switching to Wayland. Here are the steps:

## 1. Open the 61-gdm.rules file, and comment the lines that disable Wayland

```
sudo nano /usr/lib/udev/rules.d/61-gdm.rules
```

Look for the `LABEL="gdm_prefer_xorg"` and `LABEL="gdm_disable_wayland"` lines, and comment out the RUN lines with a # symbol. 

It should look like this:

```
LABEL="gdm_prefer_xorg"
#RUN+="/usr/libexec/gdm-runtime-config set daemon PreferredDisplayServer xorg"
GOTO="gdm_end"

LABEL="gdm_disable_wayland"
#RUN+="/usr/libexec/gdm-runtime-config set daemon WaylandEnable false"
GOTO="gdm_end"

LABEL="gdm_end"
```

## 2. Enable Wayland
Then, edit the `/etc/gdm3/custom.conf` file, and make sure that `WaylandEnable` is set to true:

![Image showing the WaylandEnable parameter set to true in the nano editor](/assets/wayland1.png)

## 3. Restart gdm.service
Finally, you restart the gdm.service: `systemctl restart gdm.service`. Note that this will log you out and close all programs.

## 4. Log back in, selecting Wayland for your user
Now, at the login screen, when you select your user there is a gear wheel on the bottom right side. Click it and choose "Pop on Wayland" instead of "Pop".