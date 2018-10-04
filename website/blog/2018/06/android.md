# Android
## Interesting Finds and Misc
  - [Android Studio issue checklist](https://medium.com/@mikewolfson/android-studio-is-borked-my-checklist-for-fixing-build-issues-e41a9dd8cba8)
    - A semi-ok checklist if you're having issues with your IDE although point 4 doesn't really count if you're not on Windows.
  - [Facebook Sonar](https://fbsonar.com/)
    - A cross-platform (iOS, Android) framework for visualizing and debugging data on mobile apps.
    - This was [spawned](https://fbsonar.com/docs/stetho.html) from [Stetho](http://facebook.github.io/stetho/) which is/was an Android-only framework.
  - [The dilemma of mobile apps development](https://old.reddit.com/r/androiddev/comments/8r0xo5/dilemma_of_coder/)
    - A sort-of humored but accurate analogy of one's typical x-platform vs native development journey.
    - Further in the thread there are updated versions of the image from others depicting their experiences.
    - A (more serious, detailed and super interesting) 5-part post about Airbnb's React Native x-platform journey : [React Native at Airbnb](https://medium.com/airbnb-engineering/react-native-at-airbnb-f95aa460be1c)
      - Credits to [@henk](https://twitter.com/helloserve)

## Tips and Tricks
  - [Play Store Country Targetting Support For Pre-Production Tracks](https://support.google.com/googleplay/android-developer/answer/7550024?hl=en)

    For years the Play Store country targets could only be configured as a single setting throughout the whole app.  
    This meant that if your Alpha/Beta testers were, for example, based Russia or India and the final Play Store release was only aimed for South Africa, your testers would have been unable to download the app directly from the Play Store without a few painful VPN or voodoo hacks.

    The good news is the Play Store has now been updated to allow individualized country targetting for each release track.  
    Now release management is much simpler and convenient to allow private testers from any chosen country to download from the Play Store
    and still target the final release for different set of countries.

  - [Set up an open, closed, or internal test](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en)

    You can now release early versions of your app for internal testing or to trusted users for closed and open testing, and get feedback to make improvements before full release.
