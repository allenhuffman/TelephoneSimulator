# Telephone Simulator

A browser-based simulation of a landline phone, built to recreate the phone system at Atlas9 (Kansas). It can also be used for similar systems, such as Meow Wolf locations.

Pick up the receiver, hear the dial tone, and dial a number on the keypad (on screen or keyboard). If the number is in `numbers.csv`, its recording plays. Otherwise you get the "cannot be completed" message.

It is plain HTML, CSS and JavaScript with no build step and no dependencies.

## Running

Browsers block `fetch()` on `file://`, so serve the folder over HTTP:

```
python3 -m http.server
```

Then open <http://localhost:8000>. No restart is needed after edits. Reload the page (Safari: Shift-click the reload button to bypass the cache).

## Using it

| Control | Action |
| --- | --- |
| Pick up / `Enter` | Lift the receiver, play the dial tone |
| Keypad / number keys | Dial (only works while off hook) |
| Hang up / `Esc` | Hang up, stop all audio |

`*` and `#` play their tones but do nothing else, as on a real phone. Letters are not accepted: dial digits only.

## Adding numbers

1. Drop the recording in `audio/` named after the number as written in the CSV: `553-456.mp3`.
2. Add a line to `numbers.csv`:

```
# number,description
553-456,(55-FILM) Reel Line
8000,Manager's Office Voicemail
```

- Format is `number,description`. The description is only a label, so rename it freely without touching the audio file.
- Dashes are for readability. Dialing ignores them (`553-456` is dialed as `553456`).
- Numbers can be any length. Lines starting with `#` and blank lines are ignored.
- Phone-keypad letters (such as `FILM`) go in the description, not the number. Convert them to digits (`FILM` = 3456).

## System sounds

Optional recordings go in `audio/system/`. If one is missing, a synthesized tone is used.

| File | Played when |
| --- | --- |
| `dialtone.mp3` | The receiver is picked up (looped) |
| `reorder.mp3` | Off hook and nothing dialed for too long (looped) |
| `invalid.mp3` | The number is unknown or has no recording |
| `busy.mp3` | Reserved, not triggered yet |

## Dialing rules

Settings are constants at the top of `app.js`:

| Constant | Default | Meaning |
| --- | --- | --- |
| `MAX_DIGITS` | 6 | Longest number. An unknown number is rejected once this many digits are dialed. Set it to the longest number on the system. |
| `IDLE_TIMEOUT_MS` | 5000 | Pause after which an incomplete or unknown number is rejected |
| `DIAL_TIMEOUT_MS` | 2000 | Wait after a complete number that is also the start of a longer one |
| `DIAL_TONE_TIMEOUT_MS` | 15000 | Off hook with no digits before the dial tone switches to reorder |

A complete number that no longer number starts with plays after about 0.4 seconds.

## Theming

Colors, radius and font are CSS variables at the top of `style.css`.

## Versioning and caching

The page shows `page vN / script vN` in the bottom-right corner. When you change code, bump the number in three places: `VERSION` in `app.js`, and both the page label and the `?v=` parameters on the script and stylesheet tags in `index.html`. If the two numbers on screen differ, the browser is using a cached file.

## Files

```
index.html        page
style.css         look and feel (theme variables)
app.js            dialing logic and sounds
numbers.csv       number list
audio/            one mp3 per number
audio/system/     dial tone, reorder, invalid, busy
```

## Not yet used

`audio/system/ring-reorder.mp3` is not wired up. Ringing before a number answers, or a number that rings and then fails, would use it.
