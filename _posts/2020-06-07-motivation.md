---
layout: post
---

DIY GPS Tracker is developed because I realized all current solutions require a monthly signup fee and you are locked into someone's ecosystem. My goal is to provide an open solution where you manage and own your own data. You could say that my solution is locked-in to Firebase, but you own your own data and you can export it any time. The bills you'll receive are supposed to be negligible, mostly $0 but not more than 5 cents per month.

This leaves one more monthly cost item in question: the [data SIM card](https://support.google.com/fi/thread/912102?hl=en), and that's how the whole story started. I'm a [Google Fi subscriber, so I can get data SIM cards (now only up to 4) without any monthly fees](https://www.theverge.com/2017/6/13/15782436/project-fi-data-only-sim-deal-wireless-lte-tmobile). I'll only need to pay for the transferred data, but that should be negligible. I bought a refurbished carrier independent Motorola Moto g6 phone on ebay for $30. That provides Android 9, and has USB-C charging, so relatively modern.

My main intent is theft protection. I'll hide the phone behind panels in my car, it'll be constantly hooked up to a power source. It won't be in an interactive setting though, so it's important to make sure that in case of any crash or blitz the DIYGPSTracker would be able to restart on its own without any user interaction. Hence the reason I went with the Email + Password authentication method. From a security perspective the Google authentication would be better, but in case of a restart it could be stuck with a Sign-In dialog.

Bear in mind that in my use-case the device will be hidden and locked. Even if a thief finds it and can unlock it I'm not logged in with my O.G. Google account: that would be a big mistake. For each asset I created separate Google accounts representing a child (the manufacturing year of the cars as a birth year 2016 and 2017). The accounts related to the protected cars are then also managed by [Google Family Link](https://families.google.com/familylink/) system. That could also provide an extra backup: in any case the DIYGPSTracker would fail, I could still request the location of the device using the [Family Link](https://families.google.com/familylink/) manager app.

The device just should not run out of power - that's what I should ensure. I'd also encourage you to analyze your use-case and secure it down as much as possible, and also provide a fall-back solution in case any app fails.
