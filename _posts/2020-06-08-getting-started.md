---
layout: post
---

These are the list of steps to get started with the app suite for your use-case and optionally turn on push notifications.

1. Setting up **Firebase Project**
   0. Register a Google account if you don't have one. Log in to your browser with your Google account.
   1. Go to [Firebase console](https://console.firebase.google.com/) and follow this tutorial until 1:20 (don't create iOS app).
   2. You probably need to associate some payment method to your project, don't expect too much billing though: I have more than a dozen of projects and got bills of 5 cents per month. Although I don't manage a large fleet of vehicles but only a pair of cars by DIY GPS Tracker.
   3. Register **DIY GPS Tracker** app with your Firebase project, you'll need to use `dev.csaba.diygpstracker` as a package name.
   4. Add SHA1 certificate fingerprint of 4A:1C:EE:06:88:08:3F:9E:3B:59:E9:D0:B4:E2:F5:FD:E6:D3:8A:91
   5. Add SHA256 certificate fingerprint of 3D:4D:CA:B0:31:BA:8B:D1:C6:0D:E5:D2:37:21:A0:81:C1:94:DD:C0:95:85:31:18:17:2C:89:D8:13:39:8F:ED
   6. Register **DIY GPS Manager** app with your Firebase project, you'll need to use `dev.csaba.diygpsmanager` as a package name.
   7. Add SHA1 certificate fingerprint of CF:C5:F4:A3:C0:DD:46:3F:00:A6:BE:2E:63:7B:74:28:19:96:98:F4
   8. Add SHA256 certificate fingerprint of 3D:94:4C:4E:23:0E:9D:AF:B8:C0:2A:F9:17:CF:83:11:B2:05:85:C6:87:59:39:4A:9A:18:FD:52:71:A4:FA:CD
   9. Under the _Authentication_ section of the Firebase project enable Sign-In methods you'd like to use with the app suite. For reference how to enable Google SIgn-In take a look at the [very beginning of this video](https://www.youtube.com/watch?v=UpAqd-cX9j4) Currently Anonymous, Google and Email+Password methods are supported.
   10. Under the _Database_ section activate **Firestore** feature of your Firebase project. This is _not_ the Realtime Database, it _must_ be Firestore. See [this video from 2:10](https://youtu.be/UFLvSp4Mh9k&t=130)
   11. Make sure to secure down your Firestore based on which authentication method you use with the database *Rules*. The data structure is the following: you'll have a collection of "Assets" as you add new assets with the manager. As soon as any asset will have it's DIYGPSTracker running and will start reporting the location, each asset will have its own Reports collection and eventually if any GeoFence is violated then a Notifications collection.

2. [Install DIYGPSManager](https://play.google.com/store/apps/details?id=dev.csaba.diygpsmanager) on an Android device you'll use for managing assets and supervising them
   1. [Install DIYGPSManager](https://play.google.com/store/apps/details?id=dev.csaba.diygpsmanager) from the app store (or if you compile your own then side load it).
   2. In your Firebase project select the DIYGPSManager associated app and remember the App ID and the Web API Keys.
   3. Under the settings of the application specify the **Project ID**, **App ID** and **Web API Keys** you saw.
   4. You only need to specify Email and Password if you picked Email+Password preferred authentication method otherwise it should be left blank. In case of Email+Password authentication you must not reuse passwords for any other accounts for security reasons. The app uses Jetpack Security to encrypt any preferences (for the reason for these preferences [see my motivation how the app is used]({% post_url 2020-06-07-motivation %})). If the phone is used in interactive setups you can use Google authentication which is more secure, but may require Sign-In if the app stops by any chance.
   5. The app will navigate to the Asset List view, where you can add an entry for each asset you'd like to track.
   6. You can manually override the reporting time interval, geo fence lock radius, manually lock/unlock the asset or navigate to a map view to see the asset's current location and it's trajectory in the near past.
   7. When you lock the asset the Tracker app would report back the current location and establish a geofence around that location with the given radius. Once the asset leaves that area the status icon will change from green checkmark to red, the Tracker will switch to a 10 seconds reporting interval and you may also receive a push notification.

3. [Install DIYGPSTracker](https://play.google.com/store/apps/details?id=dev.csaba.diygpstracker) on the asset devices
   1. [Install DIYGPSTracker](https://play.google.com/store/apps/details?id=dev.csaba.diygpstracker) from the app store (or if you compile your own then side load it).
   2. [Disable battery optimization](https://www.androidpolice.com/2019/07/11/how-to-disable-battery-optimization-for-google-photos-and-other-system-apps-on-oneplus-devices/) for the DIYGPSTracker app and make sure it'll stay in the foreground to avoid any limitations of the Android system. Even if battery optimization is disabled phone manufacturer's own schemes may kill of worker processes if the app is in the background, see [this reference ("Phone makers are breaking your favorite apps with reckless changes to Android's power optimization features")](https://www.androidpolice.com/2020/07/18/phone-makers-are-breaking-your-favorite-apps-with-reckless-changes-to-androids-power-optimization-features/) and [this reference ("Don't Kill My App benchmark")](https://dontkillmyapp.com/).
   3. In your Firebase project select the DIYGPSTracker associated app and remember the App ID and the Web API Keys.
   4. Under the settings of the application specify the **Project ID**, **App ID** and **Web API Keys** you saw.
   5. You only need to specify Email and Password if you picked Email+Password preferred authentication method otherwise it should be left blank. In case of Email+Password authentication you must not reuse passwords for any other accounts for security reasons. The app uses Jetpack Security to encrypt any preferences (for the reason for these preferences [see my motivation how the app is used]({% post_url 2020-06-07-motivation %})). If the phone is used in interactive setups you can use Google authentication which is more secure, but may require Sign-In if the app stops by any chance.
   6. The app should list the assets you created with the DIYGPSManager. Pick the asset which will be tracked by the device you are handling right now.
   7. After picking an asset the Tracker will display the latest location, battery level it reported to your Firestore. Stay on that view and lock the device.

4. Enable push notifications (optional, under progress)
   1. Activate Cloud Functions in your Firebase project.
   2. Clone the [https://github.com/DIYGPSTracker/DIYGPSMessaging](https://github.com/DIYGPSTracker/DIYGPSMessaging) repository.
   3. [Build and deploy the messaging cloud functions](https://firebase.google.com/docs/functions/get-started) with the following steps:
      1. `npm install -g firebase-tools`
      2. `firebase login`
      3. `firebase init functions`. Only select Cloud Functions if you are offered any other features of Firestore (Database, Hosting, ...). Select JavaScript as a language. Don't overwrite the current function.
      4. `npm install`
      5. `firebase deploy --only functions`
