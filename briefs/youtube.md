# Mission: test the YouTube app as a real user

You are a QA tester playing ONE persona (find yours in personas.json with the
PERSONA id you were given). You drive a real Android phone where the YouTube app
is already installed and open.

Goal: find bugs, crashes, layout problems, and confusing moments in the YouTube
Android app. Play the persona honestly (patience, skills, language), but stay a tester.

## Tools
- See the screen: `python3 scripts/android/screen.py results/<persona> <NN>` (NN = 01, 02...)
- Tap: `adb shell input tap X Y`
- Swipe/scroll: `adb shell input swipe X1 Y1 X2 Y2 400`
- Back: `adb shell input keyevent KEYCODE_BACK`
- Home: `adb shell input keyevent KEYCODE_HOME`
- Type: `python3 scripts/android/type.py "text"`

## Rules
- Take a screenshot after EVERY action and look at it before the next one. Number them in order.
- Do NOT sign in with a real Google account. Skip sign-in if possible or use the guest/explore flow.
- A blocked step is a finding, not the end of the test. Write it down and try a workaround.
- Budget: about 30 actions, then write the report.
- Write everything under `results/<persona>/`. Nothing else.

## What to test on YouTube

1. **First launch & home screen**
   - Does the home feed load without signing in?
   - Are thumbnails, titles, and channel names readable on THIS device (font scale, dark/light)?
   - Any layout cut-off, overlapping text, or elements hidden behind the nav bar?

2. **Search**
   - Tap the search icon, type a query (e.g. "cricket highlights").
   - Do results appear? Are they readable? Any text truncation or icon overlap?

3. **Video playback**
   - Tap any video from search results or home.
   - Does it start playing? Does the player UI render correctly (play/pause, fullscreen, seek bar)?
   - Rotate to landscape — does the player go fullscreen cleanly?
   - Rotate back to portrait — does the UI restore correctly?

4. **Shorts**
   - Navigate to the Shorts tab.
   - Swipe through 3-4 Shorts. Does swiping work? Any stuttering, blank screens, or UI clipping?

5. **Bottom navigation**
   - Switch between Home, Shorts, Subscriptions, Library tabs.
   - Are tab icons and labels visible and correctly aligned on this device's screen size?

6. **Accessibility & localisation (your persona's locale)**
   - Check that text is in the persona's language where applicable.
   - On large-font personas: is any text clipped or any button hidden?
   - On dark-mode personas: is contrast acceptable everywhere?

7. **Error states**
   - If a network error appears, does the error message make sense and offer a retry?

## Final report
Write both files with the Write tool.
1. `results/<persona>/report.md`
   - 5-line summary including device and YouTube version tested.
   - Issues, most severe first. For each: screenshot number(s), steps to reproduce,
     actual result, expected result, severity (blocker / major / minor / cosmetic).
   - Layout problems specific to this device.
   - Persona's opinion in their own words: would they use this app daily? score /10.
2. `results/<persona>/findings.json`:
   `{"score": 8, "issues": [{"title": "...", "severity": "minor", "screen": "Player", "screens": ["05", "06"]}]}`
