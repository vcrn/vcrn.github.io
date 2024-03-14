+++
title = "Using GrapheneOS, the private and secure mobile OS"
date = 2022-11-14

[taxonomies]
tags = ["GrapheneOS", "Privacy", "Security"]
+++

Some weeks back, I bought a Pixel 6a and installed GrapheneOS on it. GrapheneOS is a private and security hardened version of Android, and so far I've had zero issues. This post will be a very brief
description of the OS, and where you can find more information about it.
<!-- more -->
# What to expect 
GrapheneOS comes with the absolute minimum of apps, so none of the usual bloatware that can't be uninstalled is included, such as Facebook, Samsung App Store, etc. 
Google Play Store and its related privacy invasive services isn't even installed, but can easily be installed in a sandboxed manner that severely limits its permission, 
stopping it from sending data to Google as it pleases.

Something that stands out when using GrapheneOS in comparison with standard Android is ability and need to give explicit permission for apps to access storage, network, 
camera, etc. In practice, this means that you for example can shut off all network permission for a keyboard app, making it unable to send data of your keystrokes to its developers. 
All the apps are much more strictly handled, and it's in this same manner that Google Play and its Services are handled, no longer giving them unrevocable privileged access to do what they want. 

It also comes with toggles to completely shut off Location services, microphone or camera of the device, just as you're normally able to turn on and off Bluetooth. 
This is just the absolute tip of the iceberg, for information, you can read [here](https://grapheneos.org/features). 

If you want to be stricter but at the same time need access to apps that you'd prefer not to use, you can use User profiles: That way, the apps on the different profiles can be kept completly separate,
without being able to access the other User profiles data.

# PrivSec: Security enables privacy
An insecure phone that isn't dependent on privacy violating companies neither secure nor private: with an insecure device, you are at a higher risk of being monitored by other parties. 
Security is thus of utmost importance, and if all ties to privacy violating companies have been severed at the cost of security, both security and privacy has been harmed.

# Supported hardware
It might seem a bit ironic to buy a Google phone to get away from Google, which I thought at first too. There are several reasons for this: 
It so happens to be that the Pixel phones are the only phones that support changing the operating sysyem while still letting the user lock the bootloader and use the full hardware security, 
open-sourcing their firmware, being very secure hardware-wiseand much more. On top of this, they are very secure hardwarewise. [Here's](https://www.youtube.com/watch?v=WkQ_OCzuLNg) 
a great interview with one of the developers of GrapheneOS going into further details about why they only support the Pixel phones, and other details of the OS.

# App compatability
What previously stopped me from moving to alternatives to Android and iOS was incompability with everyday apps, specifically banking apps. 
Here's a page that lists the compatibility of individual apps related to banking, identification, etc.

# Support
If you use, or just are fan of the project, please consider supporting the project financially [here](https://grapheneos.org/donate) or via code contribution.
