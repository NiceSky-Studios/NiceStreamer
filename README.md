# NiceStreamer

A light, native macOS app for TikTok LIVE. Sound effects, text-to-speech, and a like counter with a goal overlay, without the heavy GPU use.

**Version 1.1.0**

## What it does

- **Sound effects.** Play a clip when someone follows, shares, sends a gift, types a chat command (like `!airhorn`), or when your likes pass a milestone.
- **Text-to-speech.** Read chat messages aloud, and announce new followers, gifts and goal completions, using any voice installed on your Mac.
- **Like counter and goal.** Track likes for your session and set a goal that can raise itself when you hit it.
- **Goal on stream.** Show your goal bar with a local link, a public https link, or a goal window you can capture.
- **Built to stay light.** No blur, no animations, and the overlay only updates when your numbers change.

## Requirements

- macOS 13 or newer
- [Node.js](https://nodejs.org) 18 or newer (the part that reads your live room uses it)
- An [Euler Stream](https://www.eulerstream.com) API key (see below)
- Optional: [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) for the public https link (`brew install cloudflared`)

## Getting started

1. Open NiceStreamer.
2. On **Dashboard**, paste your **Euler Stream API key** into the banner at the top and press **Save**.
3. Type your TikTok username (without the @) and press **Connect**.
4. Add a few sounds on **Sounds**, pick a voice on **Voice**, and set your goal on **Goal & Overlay**.

Not live yet? The **Try it without being live** buttons on the dashboard let you test sounds, speech and the goal bar any time.

## About the API key

NiceStreamer reads your live room through a connection library, and that library gets its connection signed by the Euler Stream service. Without a key you share a limited community quota, which is often used up, so connecting can fail. Adding your own key fixes that in most cases. The key is only used for connecting. Text-to-speech and sounds run entirely on your Mac.

You can set or change the key on the Dashboard banner or on the **Connection** page. It is stored in your settings file on your Mac.

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

## Showing your goal on stream

There are three ways to get the goal bar onto your stream. Try them in this order.

### 1. Local link (OBS and similar apps)

On **Goal & Overlay** you'll find a link like this:

```
http://127.0.0.1:8787/goal
```

Add it as a **web / browser source** (suggested size **800 × 120**, transparent background). It works as soon as NiceStreamer is open, even before you connect.

Some apps, including TikTok LIVE Studio, won't accept an `http://` or localhost address. If yours doesn't, use option 2 or 3.

### 2. Public https link

Turn on **Create a public https link** on **Goal & Overlay**. NiceStreamer uses a free Cloudflare Quick Tunnel (through `cloudflared`) to give your overlay an `https://…trycloudflare.com/goal` address that any app can use.

- The address **changes every time NiceStreamer starts**, so paste the new one into your streaming software each session.
- Anyone who has the link can see your goal bar (the title and the numbers, nothing else).
- If it says cloudflared isn't installed, run `brew install cloudflared` in Terminal and switch the option off and on.

### 3. Goal window

Click **Open goal window** on **Goal & Overlay** and add that window as a **window capture** source. Pick **Green screen** for the background and apply a chroma key filter in your streaming software to make it see-through. This needs no link at all.

### Other options

- **Different computer?** If your streaming software runs on another computer, turn on **Allow other devices on my network** and use the second local link shown. Leave it off otherwise, since anyone on your Wi-Fi could open the link while it's on.
- **Port in use?** Change the port in the overlay options.
- **Text sources.** NiceStreamer also writes `goal.txt` and `likes.txt`. Use **Show data folder in Finder** to find them.

## TikTok LIVE Studio

TikTok LIVE Studio has no public API, so NiceStreamer can't plug into it directly. Instead it reads your live room by username, which works whichever software you stream from. On the **Connection** page you can turn on **Connect when LIVE Studio opens**, which connects and disconnects automatically when an app named "TikTok LIVE Studio" is running on the same Mac.

## Getting sounds and speech into your stream

NiceStreamer plays audio through your Mac's output. To get it into your stream, route your Mac's audio into your streaming software, for example with a loopback audio device such as BlackHole.

## Troubleshooting

| Problem | Try this |
| --- | --- |
| Won't connect | Add an Euler Stream API key (Dashboard banner or Connection page), make sure you're live, and check the username |
| "Node.js not found" | Install Node.js, or enter its path on the Connection page |
| Streaming app rejects the overlay link | Use the public https link or the goal window |
| "cloudflared isn't installed" | Run `brew install cloudflared`, then switch the public link off and on |
| Overlay link doesn't load | Check the status on Goal & Overlay; if the port is busy, pick another one |
| No sound on stream | Route your Mac's audio output into your streaming software |
| Speech never plays | Check the Voice page and that Text-to-speech is on in Quick controls |

## Updates

NiceStreamer checks for new versions on [GitHub](https://github.com/NiceSky-Studios/NiceStreamer/releases) when it opens (at most every few hours).

- When a new version is out, a banner appears on the **Dashboard** and the sidebar shows an **Update** link.
- Press **Update now** to download it, replace the app and reopen it. NiceStreamer never updates or restarts by itself, because that would cut your sounds and overlay mid-stream.
- The **About** page shows your version, a **Check now** button, the release notes for the new version, and a switch to turn automatic checking off.
- **Skip this version** hides the banner for that version until the next one comes out.
- If NiceStreamer can't replace itself where it is installed, it downloads the new version and shows it in Finder so you can drag it into your Applications folder.

## Where your data lives

Settings and imported sound files are stored in `~/Library/Application Support/NiceStreamer`.

## Good to know

- The connection to TikTok is **unofficial**. It can stop working when TikTok changes things, until the underlying library is updated.
- NiceStreamer is an independent project and is not affiliated with or endorsed by TikTok, ByteDance or Euler Stream.
