# Honk Tonk on Google Play

What to enter in Play Console for the current build, and what to change
before it goes live. It matches the game as it is now: no ads, no purchases,
no data collected.

## Before the first upload

1. **Fill in the privacy policy.** Replace `[DEVELOPER NAME]` and
   `[CONTACT EMAIL]` in `store/privacy-policy.html`, host the page, and paste
   its address in Play Console → App content → Privacy policy. Put the same
   address in `PRIVACY_URL` in `scripts/ui/about_screen.gd`.
2. **Create your upload key** (once): `bash tools/make_upload_key.sh`. You
   choose the password; keep the file and password backed up.
3. **Build the release:** `bash tools/build_play.sh` writes a signed
   `build/honk-tonk.aab`. Play App Signing (on by default) keeps the app's
   signing key; your key only proves uploads come from you.
4. **Closed test first.** New personal developer accounts must run a closed
   test with at least 12 testers for 14 days in a row before applying for
   production access.
5. **Bump the version** (`version/code` and `version/name` in
   `export_presets.cfg`, both presets) before every upload.

## Store listing

- **App name:** `Honk Tonk` (or `Honk Tonk: Traffic Light Songs`, exactly 30 of 30 characters).
- **Short description (80 max):** Time the traffic lights so rush hour never stops, and the city plays a song.
- **Full description:**

  > Every car in this city is an instrument: red drums, blue bass, yellow
  > piano, green marimba. Each one plays its note as it rolls over a
  > crosswalk. Your job is the traffic lights. Tap a light to change which
  > street gets green on each beat, then press Play and watch rush hour roll.
  >
  > Get it right and nobody stops: the traffic plays a song, and it keeps
  > looping while you enjoy it. Get it wrong and a car has to wait, honking
  > off the beat, and the one behind it crunches into its bumper.
  >
  > - No timers and no reflexes: set the lights, then watch
  > - Every light affects two streets at once, so each city is a real puzzle
  > - Step through rush hour one beat at a time to see exactly who stopped
  > - Cities grow from one crossing to eight
  > - A song of the day, the same city for everyone
  > - A daily bonus streak and a gift every five levels
  > - Every instrument has its own symbol, not just a colour
  > - No ads, no accounts, no data collected

- **Category:** Puzzle (or Music). **Tags:** puzzle, music, logic, casual.
- Don't use other games' names in the name, descriptions or keywords. Google
  treats that as impersonation or keyword spam, even for descriptive-sounding
  names. Run a trademark search on "Honk Tonk" before launch.
- Screenshots must show the real game: use `tests/screenshots.tscn`, or a
  phone.

## App content answers (current build)

| Question | Answer |
|---|---|
| Privacy policy | The hosted `store/privacy-policy.html` |
| Ads | No, the app doesn't contain ads |
| App access | All functionality is available without special access |
| Content rating (IARC questionnaire) | Game; cars bump into each other comically (a crunch sound, dizzy eyes, nobody is hurt), which the questionnaire treats as no violence or at most mild cartoon action; no fear, sexuality, language, controlled substances, gambling or simulated gambling; no user interaction or chat; no shared location; no digital purchases. Expect Everyone / PEGI 3 |
| Target audience | See below |
| Data safety: collects or shares any user data? | **No** |
| Data safety: encrypted in transit / deletion request | Not applicable (nothing is collected) |
| Government, financial, health, news app | No |

**Target audience.** Pick the ages honestly. If you include children under
13, Google's Families policy applies: any ad SDK you add later must be
Families-certified, ads to children can't be personalized, and the store
listing must suit children. Honk Tonk's cartoon style may count as appealing
to children even if you choose 13+, so say in the listing that it's made for
teens and adults if that's your choice. With no ads and no data today, either
choice is simple; it matters most once ads go in.

## When you add ads or purchases

The code is ready for it: everything goes through `scripts/autoload/ads.gd`.
Release builds show no ads and sell nothing until you wire real plugins in.
An empty Fix costs coins instead.

1. Install an AdMob plugin (Poing Studios', Godot 4.5+, or
   `godot-sdk-integrations/godot-admob`, Godot 4.7+) and Google Play Billing.
2. Set the modes to `LIVE` in `ads.gd` and route `show_rewarded`,
   `show_interstitial` and the Remove ads purchase to the plugins.
3. Show Google's consent form (UMP) in the EEA, UK and Switzerland, and add a
   "Privacy options" button in settings.
4. Host `app-ads.txt` on your developer website.
5. Update the privacy policy (Lead Astray's ads version in its `store/` folder
   is a good starting point), then Play Console: Ads → Yes, and Data safety →
   device or other IDs, app activity (ad interactions), diagnostics, shared
   with Google for advertising, fraud prevention and analytics.
6. If you ever sell random rewards, Google requires the odds to be shown
   before purchase. Today's gifts and bonuses are free and fixed.

## Sharing APKs outside Play

From 30 September 2026, certified Android phones in Brazil, Indonesia,
Singapore and Thailand only install apps from verified developers, including
APKs shared directly; the rest of the world follows from 2027. Installing over
USB with adb still works. Verify your identity in Play Console or the Android
Developer Console before sending APKs to testers there.
