---
title: "PowerButton App: How to use a phone with a completely dysfunctional power button"
---

# Side note

This happened around 2019, now I've actually fixed the power button itself, this is more of a workaround and it doesn't actually allow you to do everything that you can normally do with a working power button. 
I actually had other problems with the phone since then but I fixed it again (motherboard replacement, which is expensive) and I'm still using it.

# The problem

I've owned my current phone, a *OnePlus 3*, a for now more than **5 years**. At the time of the problem it was more like **3 years**. 

I'm not going to expand on it here but this is an excellent phone, which is still very usable in 2021.

Anyway, 2 years ago approximately, the power button stopped working gradually. I would have to click harder and harder on it (sometime I pressed the phone on the corner of table very hard to click it ), until it stopped working completely at some point.

This means that :
1. While turned on, I could not bring up the power menu 
2. While turned on, I could not take screenshot, because they require you to press **volume down** and the **power button** at the same time.
3. While turned on, I could not lock the screen (you can actually unlock the screen if you have fingerprint registered)
4. I could turn on the phone but only following a complicated procedure (holding the volume **down** key and plugging the charger goes to **recovery mode**, from that you reboot in normal mode)
5. I could not boot into specials boots modes other than **recovery mode** (yes I can reach other mode from this mode, but what if this mode is bricked)

# Solutions for some problems

<span style="color:green">3) There are multiple native solution to this problem, the simplest one being setting the screen timeout to a low value so that it turns off itself. In my case I used a feature of LineageOs that locks the screen when you double tab the notification area (or the home button).</span>

<span style="color:green">4) Was not actually a problem because I could actually turn on the phone, even tough it's a was annoying procedure.</span>

<span style="color:orange">5) Could definitively be a problem in case the phone get bricked and by recovery mode doesn't work I can't use fastboot. Right now I can access it whithout a power button because I have a working recovery mode...</span>

1) and 2) Surprisingly where the only unfixable problem. I did not find any existing way to trigger the power menu to appear or to take screenshot without a power button (on Lineage Os anyway) (and I did look around a lot). 

But you can actually take screenshots from the power menu, so if I can trigger the power menu then I can also take screenshot.

| ![screenshot of the android power menu](https://www.androidcentral.com/sites/androidcentral.com/files/styles/large_wm_brw/public/article_images/2020/06/android-11-device-controls-2.jpg) | 
|:--:| 
| *The android power menu in the most recent version of android (not from me) * |

# Possibles solutions to make the power menu appear

Of course the best solution, and the recommended solution, is to fix the underlying hardware problem, which in that case is actually very cheap because it's just a button.

Other than than, I actually found an [App](https://play.google.com/store/apps/details?id=io.github.visnkmr.powermenu&hl=fr&gl=US&showAllReviews=true), that works but: 
- **This app did not exist at the time I had the problem**
- This app is **closed source**
- This app doesn't allow you to trigger the power menu anywhere, only when you are in the app, so you can't use it to take screenshot...

# My solution : an Application that use a quick setting tile to trigger the power menu.

If you have used a smartphone in the last 5 years you know what quick settings are. They allow you toggle features like Wifi and Bluethooth very quickly for example.

| ![screenshot of the android quick settings](https://devblogs.microsoft.com/wp-content/uploads/sites/44/2019/03/QuickSettings.png) | 
|:--:| 
| *Quick settings in Android 7.0 (not from me)* |

My idea was then to add a quick here to trigger the power menu. But how can you do that ?

## The shell command that emulates button presses

First, after many hours of research, I found that there is in android a shell command that allows you to emulate key presses. Unfortunately, you need to be `root` for this command to work. 

| ![screenshot highlighting the back button](https://storage.googleapis.com/images.zoftino.com/development/android/android_back_button.png) | 
|:--:| 
| *For example the command `input keyevent KEYCODE_BACK` will emulate the click of this button* |

If you have a rooted android device you try that in a root shell, it should work.

## The quick settings "API"

Any app developer has the possibility of adding new tiles to the quick settings by extending the `TileService` class

Then all you have to do (there are other things you can do as well to act on the look of the custom tile) is to `@Override` the `onclick()` method.

You can then call a shell command in java directly by using `Runtime.getRuntime().exec("<shell command>")`

Now the shell command will be called every time you click the tile.

First I'm calling `input keyevent KEYCODE_BACK` so that it clears the quick settings and notification area.

And then I call the `input keyevent --longpress 26`. 26 here corresponds to the power button (it might be different an another phone)

And that does make the power menu appear: success ! 

| ![screenshot of the custom quick tile](https://raw.githubusercontent.com/olimar718/powerbutton/master/InkedScreenshot_20190603-130747_Trebuchet_LI.jpg) | 
|:--:| 
| *That's how it looks like when it's installed* |


{% include footer.md %} 