# NiceStreamer

A light, native macOS app for TikTok LIVE. Sound effects, text-to-speech, and a like counter with a goal overlay, without the heavy GPU use.

**Version 1.0.0**

## What it does

- **Sound effects.** Play a clip when someone follows, shares, sends a gift, types a chat command (like `!airhorn`), or when your likes pass a milestone.
- **Text-to-speech.** Read chat messages aloud, and announce new followers, gifts and goal completions, using any voice installed on your Mac.
- **Like counter and goal.** Track likes for your session and set a goal that can raise itself when you hit it.
- **Goal overlay link.** A link you can paste into TikTok LIVE Studio or OBS as a web/browser source to show your goal bar on stream.
- **Built to stay light.** No blur, no animations, and the overlay only updates when your numbers change.

## Requirements

- macOS 13 or newer
- [Node.js](https://nodejs.org) 18 or newer (the part that reads your live room uses it)
- A TikTok account that is live (or about to be)

## Getting started

1. Open NiceStreamer.
2. On **Dashboard**, type your TikTok username (without the @) and press **Connect**.
3. Add a few sounds on **Sounds**, pick a voice on **Voice**, and set your goal on **Goal & Overlay**.

Not live yet? The **Try it without being live** buttons on the dashboard let you test sounds, speech and the goal bar any time.

## Sounds

Each sound is a rule: a trigger, an audio file, a volume and a cooldown.

| Trigger | What it does |
| --- | --- |
| New follower | Plays when someone follows you |
| Share | Plays when someone shares your stream |
| Gift | Plays for a specific gift (type its name), or any gift if left blank |
| Chat command | Plays when a chat message is exactly your command, like `!airhorn` |
| Like milestone | Plays every N likes (for example, every 100) |

Short clips (under about 10 seconds) work best. The cooldown stops a sound from spamming during busy moments.

## Voice

- Choose which events are read aloud: chat, followers, gifts, goal reached.
- Set the voice, speed and volume.
- Messages are trimmed to a maximum length, links and emoji are removed, and anything containing a blocked word is skipped.
- If chat gets busy, extra messages are dropped instead of building up a backlog.

## Goal and overlay link

On **Goal & Overlay** you'll find your overlay link, which looks like this:

```
http://127.0.0.1:8787/goal
```

To show it on stream, add a **web / browser source** in TikTok LIVE Studio or OBS and paste the link. A source size of about **800 × 120** works well, and the background is transparent. The link works as soon as NiceStreamer is open, even before you connect.

- **Different computer?** If your streaming software runs on another computer, turn on **Allow other devices on my network** and use the second link shown. Leave it off otherwise, since anyone on your Wi-Fi could open the link while it's on.
- **Port in use?** Change the port in the overlay options.
- **Text sources.** NiceStreamer also writes `goal.txt` and `likes.txt` if you'd rather use a plain text source. Use **Show data folder in Finder** to find them.

## TikTok LIVE Studio

TikTok LIVE Studio has no public API, so NiceStreamer can't plug into it directly. Instead it reads your live room by username, which works whichever software you stream from. On the **Connection** page you can turn on **Connect when LIVE Studio opens**, which connects and disconnects automatically when an app named "TikTok LIVE Studio" is running on the same Mac.

## Getting sounds and speech into your stream

NiceStreamer plays audio through your Mac's output. To get it into your stream, route your Mac's audio into your streaming software, for example with a loopback audio device such as BlackHole.

## Connection settings

- **Node.js path.** Leave blank and NiceStreamer will find Node.js itself. Set it only if auto-detect fails.
- **Euler Stream API key (optional).** If connecting is rate-limited or fails, a free API key from [eulerstream.com](https://www.eulerstream.com) usually fixes it. The key is stored in your settings file.

## Troubleshooting

| Problem | Try this |
| --- | --- |
| "Node.js not found" | Install Node.js, or enter its path on the Connection page |
| Can't connect to your live | Make sure you're live, check the username, and try adding an Euler Stream API key |
| Overlay link doesn't load | Check the status on Goal & Overlay; if the port is busy, pick another one |
| No sound on stream | Route your Mac's audio output into your streaming software |
| Speech never plays | Check the Voice page and that Text-to-speech is on in Quick controls |

## Where your data lives

Settings and imported sound files are stored in `~/Library/Application Support/NiceStreamer`.

## Good to know

- The connection to TikTok is **unofficial**. It can stop working when TikTok changes things, until the underlying library is updated.
- NiceStreamer is an independent project and is not affiliated with or endorsed by TikTok or ByteDance.
