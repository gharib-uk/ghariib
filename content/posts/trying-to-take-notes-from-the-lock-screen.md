---
title: "(Trying to) take notes from the lock screen"
date: 2025-01-10
---

All the way back in early 2024, Android Authority wrote that in a future update Google Keep might let you jot notes from the lock screen of an Android tablet. The Android 14, Android 14-QPR1, and Android 14-QPR2 release notes mention that the

> `NOTES` role in Android 14 supports the note-taking feature and increases productivity of Android tablets. With the `NOTES` role, OEMs can give the end users a consistent note-taking experience when using a stylus on an Android tablet on the users' preferred note-taking app. For more details, see `NOTES` on Android Roles.

While that sounds great, why should this feature be limited to tablets? I believe being able to quickly and easily jot down notes on the go would significantly boost productivity. My Surface Duo supports stylus input. And with the DUO-DE custom ROM, it runs Android 15, so it should be well prepared for a quick test drive, right?

Well. Without any preparations the _Default apps_ settings page looked like this:

![Default apps settings page](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fu92rtmvg4ln8d5gbed7y.png)

Nothing related to note taking. However, in _Developer_ options, we can force enable the `Notes` role.

![Developer options](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcppb6awg1q6ruku71ve3.png)

Once I activated _Force enable Notes role_ and restarted the device a few times, a new entry appeared in _Default apps_. And, even better, _Keep Notes_ was not only listed but could even be selected as the default notes app:

![The Default apps settings page, this time showing a Notes app entry](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcmzui9988wsflb86ylqz.png)

![Setting the default notes app](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7a77fv3yvajximms98gm.png)

The final step is to add a shortcut to the lock screen.

![Adding a shortcut to the lock screen](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fu57xnv8aaeqyzzb028hh.png)

While it looked like I had set everything up correctly, the lock screen just did not show a _Note taking_ icon. Changing postures had no effect. Neither did tapping on the screen with a stylus help. Being a developer, I was curious to learn what might be going on. But how do we debug system features, particularly if we are not too deep into AOSP internals?

### Keyguard Quick Affordances

The first question that occurred to me was _How do these lock screen icons work?_ Buried deep inside _android.googlesource.com_, there is a very interesting document titled Keyguard Quick Affordances.

> To implement a new Quick Affordance, a developer may add a new implementation of `KeyguardQuickAffordanceConfig` in the `packages/SystemUI/src/com/android/systemui/keyguard/data/quickaffordance` package/directory and add it to the set defined in `KeyguardDataQuickAffordanceModule`.

So, we should find a reference to note taking in `packages/SystemUI/src/com/android/systemui/keyguard/data/quickaffordance`, right? Let's take a look.

![Contents of directory packages/SystemUI/src/com/android/systemui/keyguard/data/quickaffordance on the main branch](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Faxhwe8uo5spla2thfmog.png)

While the `main` branch currently does not contain code related note taking, it apparently did on Jan 19, 2023, until it was merged into `tm-qpr-dev`.

![Screenshot showing the merged files](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fi8q9qy1ypbsk0upv0o86.png)

Now, considering these files, can we find out why we can add the _Note taking_ icon but not use the feature in the end?

![Source code of NoteTaskModule.kt](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3xg6fnjp1ahopohan24c.png)

As far as I understand it, `NoteTaskModule` decides inside `provideIsNoteTaskEnabled()` whether to enable note taking or not. Now, given my setup, `roleManager.isRoleAvailable(NoteTaskIntentResolver.NOTE_ROLE)` should evaluate to `true`. What about `featureFlags.isEnabled(Flags.NOTE_TASKS)`? Doing an Android Code Search on `Flags.NOTE_TASKS` shows this piece of code:

![Result of an Android Code Search on Flags.NOTE_TASKS](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8oe566hm6bvnvq7pm394.png)

Invoking `releasedFlag()` returns an instance of `ReleasedFlag`, which, according to the documentation, is `true` by default. If the value is not changed to `false` at some point, both conditions inside `provideIsNoteTaskEnabled()` are met. And this kind of makes sense, because kindly recall that I could add the note taking icon in the lock screen settings. It just won't be visible on the lock screen.

Let me mention one more class, NoteTaskIntentResolver. It is

> responsible to query all apps and find one that can handle the `ACTION_CREATE_NOTE`

How about taking its code and adding it to an ordinary app? This should simplify debugging. If _Google Keep_ supports the `Notes` role, it should be returned here, right? Well, let's see.

Here are two slightly adapted versions of `resolveIntent()` and `resolveActivityNameForNotesAction()`.  

```
fun resolveIntent(): Intent? {
  val intent = Intent(ACTION_CREATE_NOTE)
  val resolveInfoFlags = ResolveInfoFlags.of(
      PackageManager.MATCH_DEFAULT_ONLY.toLong()
  )
  for (info in packageManager.queryIntentActivities(
      intent, resolveInfoFlags
  )) {
      val packageName = 
        info.activityInfo.applicationInfo.packageName
        ?: continue
      val activityName =
        resolveActivityNameForNotesAction(packageName)
        ?: continue
      return Intent(ACTION_CREATE_NOTE)
        .setPackage(packageName)
        .setComponent(ComponentName(packageName, activityName))
        .addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
  }
  return null
}

private fun resolveActivityNameForNotesAction(
  packageName: String
): String? {
  val intent = Intent(ACTION_CREATE_NOTE).setPackage(packageName)
  val resolveInfoFlags = ResolveInfoFlags.of(
        PackageManager.MATCH_DEFAULT_ONLY.toLong()
  )
  packageManager.resolveActivity(
    intent,
    resolveInfoFlags
  )?.activityInfo?.let { activityInfo ->
    if (activityInfo.name.isNullOrBlank()) return null
    if (!activityInfo.exported) return null
    // FLAG_SHOW_WHEN_LOCKED marked as hidden
    if (activityInfo.flags and 0x800000 == 0) return null
    // FLAG_TURN_SCREEN_ON marked as hidden
    if (activityInfo.flags and 0x1000000 == 0) return null
    return activityInfo.name
  }
  return null
}
```

If you want to write a note taking app that can be summoned from the lock screen, `resolveActivityNameForNotesAction()` nicely tells you how your activity should be configured.

### Wrap up

At this point, I hope you are eager to learn if _Keep Notes_ showed up here.

It. Did. Not.

That came as a surprise. Afterwards I checked _Standard apps_ and found out that _Keep notes_ no longer appears to be a candidate for the `Notes` role. While I don't want to speculate, a reasonable explanation might be that an update (in fact, I received one after I took the screenshots you saw above) deactivated the feature for some devices or device categories. Which would be really sad. As I mentioned at the beginning, limiting the ability to start note taking apps from the lock screen to tablets (or vendors that choose to support it) feels pretty short-sighted. And, no, I don't want to say _Hey Gemini, please open the Notes app_. I just want a quick way to open that app. Period. For now I consider this a prime example of a lost opportunity.

So, at least on my Surface Duo I can't take notes from the lock screen. Did you have some luck on your device? Please share your thoughts in the comments.

Go to Source
