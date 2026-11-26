# NiceStreamer

A light, native macOS app for TikTok LIVE. Sound effects, text-to-speech, chat commands, a like goal overlay and stream recaps, without the heavy GPU use.

**Version 1.4.0**

## What it does

- **Sound effects.** Play a clip when someone follows, shares, joins, sends a gift or a gift combo, types a chat command, or when your likes pass a milestone.
- **Text-to-speech.** Read chat aloud and announce followers, gifts, combos, joins and goals, with a short list of good Mac voices or natural Google voices.
- **Chat commands.** Viewers type something like `!goal` and NiceStreamer replies out loud, in your TikTok chat, or both. Commands can also be limited to you or your moderators and can control NiceStreamer itself.
- **Timed announcements.** Repeat a reminder every few minutes while you're live.
- **Like counter and goal.** Track likes and set a goal that can raise itself.
- **Goal on stream.** Show the goal bar with a local link, a public https link, or a goal window you can capture.
- **Stream recap.** Totals for each stream, saved as a file when it ends.
- **Menu bar and shortcuts.** Control NiceStreamer without opening the window.
- **Audio output picker.** Send sounds and speech to a specific device.
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

NiceStreamer reads your live room through a connection library, and that library gets its connection signed by the Euler Stream service. Without a key you share a limited community quota, which is often used up, so connecting can fail. Adding your own key fixes that in most cases. The key is only used for connecting. Sounds and speech run entirely on your Mac.

You can set or change the key on the Dashboard banner or on the **Connection** page. It is stored in your settings file on your Mac.

## Sounds

Each sound is a rule: a trigger, an audio file, a volume and a cooldown.

| Trigger | What it does |
| --- | --- |
| New follower | Plays when someone follows you |
| Share | Plays when someone shares your stream |
| Viewer joins | Plays when someone joins (use the cooldown so busy rooms don't turn into noise) |
| Gift | Plays for a specific gift (type its name), or any gift if left blank |
| Gift combo | Plays when one gift streak reaches a minimum size you choose |
| Chat command | Plays when a chat message is exactly your command, like `!airhorn` |
| Like milestone | Plays every N likes (for example, every 100) |

Short clips (under about 10 seconds) work best.

## Chat commands

On the **Commands** page, each command has a trigger (like `!goal`), a reply, and options for how it behaves.

- **Say aloud** reads the reply with your chosen voice. **Post in chat** sends it to your TikTok chat (needs chat replies turned on, see below).
- **Who can use it:** everyone, moderators and you, or only you. You're always recognised by your username. Moderator detection is best effort.
- **Action:** a command can also run something, such as skip the current speech, mute or unmute sound effects, mute or unmute speech, or reset the like counter. This is how you build streamer-only commands like `!skip` and `!mute`. The reply can be left empty for an action-only command.
- **Cooldowns:** one for everyone and one for each viewer, so a command can't be spammed.
- A command matches the first word of a message, so `!shoutout someone` triggers `!shoutout` and `{args}` becomes `someone`.
- Command messages aren't read aloud as normal chat.
- Press **Add examples** on an empty page to start with `!goal`, `!hello`, `!skip` and `!mute`, and **Test** on any command to try its reply without being live. Tests never post to chat or run actions.

### Variables

Use these in replies and announcements and they're filled in automatically:

| Variable | Meaning |
| --- | --- |
| `{user}` | The viewer who typed the command |
| `{args}` | Anything they typed after the command |
| `{likes}`, `{target}`, `{goal}` | Your like count, goal target and goal title |
| `{percent}`, `{remaining}` | Goal progress and likes left |
| `{viewers}` | Viewers watching right now |
| `{diamonds}` | Diamonds received this stream |
| `{followers}` | New followers this stream |
| `{topgifter}` | This stream's top gifter |
| `{lastfollower}`, `{lastgifter}` | The most recent new follower and gifter |

### Timed announcements

Also on the **Commands** page. Each announcement repeats every N minutes while you're connected, and can be spoken, posted in chat, or both. The first one comes after a full interval.

### Posting replies in TikTok chat

Posting in chat means NiceStreamer has to act as your logged-in TikTok account, so it's off by default. To turn it on:

1. On the **Connection** page, switch on **Post replies in chat**.
2. In your browser, log in to tiktok.com, open the developer tools, and find the cookies for tiktok.com (under the Application or Storage tab).
3. Copy the values of **sessionid** and **tt-target-idc** into NiceStreamer and press **Save login**.
4. Make sure your Euler Stream API key is set, then turn on **Post in chat** for the commands and announcements that should post.

Things to know before you do:

- Your cookies are stored in the macOS Keychain, not in the settings file. **Remove login** deletes them.
- While chat replies are on, your session is sent to the Euler Stream signing server (`api.eulerstream.com`) in order to sign the connection. NiceStreamer allow-lists that one host for you, because the connection library refuses to send a login anywhere you haven't approved.
- Automated posting may go against TikTok's rules, so use it at your own risk.
- NiceStreamer posts at most one message every 2 seconds and trims messages to 150 characters.
- macOS may ask for permission to use the Keychain item again after an update.

## Voice

- Choose which events are read aloud: chat, followers, gifts, goal reached, and optionally viewer joins and gift combos.
- **Joins:** write your own welcome message (use `{user}`) and set how often at most one is spoken, so a busy room doesn't turn into noise.
- **Combos:** a gift streak at or above the combo size you choose gets its own call-out.
- Set the voice, speed and volume.
- The voice list is kept short: good voices in your language and English, best quality first, with the novelty voices left out. Turn on **Show every installed voice** to see them all.

### Google voices

Switch the **Voice engine** to **Google voices** for the clear, familiar Google sound. A Mac has no built-in Google voice, so this uses Google Cloud Text-to-Speech with your own API key.

1. In Google Cloud, enable the Text-to-Speech API and create an API key (the **Get a key** button opens Google's guide).
2. Paste the key on the **Voice** page and press **Save key**. It's stored in your Keychain.
3. Pick a voice from the short list (US, UK and Australian English, male and female), or type any Google voice name, like `es-ES-Standard-A`, to use another language.

Things to know:

- The text of each message is sent to Google to be spoken, including what your viewers type.
- Google gives a monthly free allowance and may charge beyond it, so check its pricing.
- If Google can't be reached or the key is wrong, NiceStreamer tells you and speaks with your Mac voice instead, so nothing is lost.
- Google voices follow your speed and volume settings and play on the speech output device you chose in **Settings**.
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

- **Different computer?** Turn on **Allow other devices on my network** and use the second local link shown. Leave it off otherwise, since anyone on your Wi-Fi could open the link while it's on.
- **Port in use?** Change the port in the overlay options.
- **Text sources.** NiceStreamer also writes `goal.txt` and `likes.txt`. Use **Show data folder in Finder** to find them.

## Stream recap

NiceStreamer counts likes, gifts, diamonds, new followers, shares, chat messages and peak viewers for each stream, plus your top gifters. The **Dashboard** shows the live numbers, and the **Recap** page has all of it.

- A stream starts when you connect and ends when you disconnect or TikTok ends the stream. A dropped connection that comes back counts as the same stream.
- When a stream ends, a recap is saved as a text file you can open anywhere. Past recaps are listed on the **Recap** page.
- **End stream & save recap** saves one on demand and starts a fresh count.
- By default the like counter resets when a new stream starts. You can turn that off in **Settings**.

## Audio output

On **Settings**, choose where sound effects and speech play, for example a loopback device that feeds your streaming software. This gets your sounds into the stream without routing everything your Mac plays.

- Sound effects play straight to the chosen device.
- Speech to a chosen device is made with the macOS `say` tool and then played, so it can start a moment later. Speech on the system default uses the normal speech engine.
- If a chosen device is unplugged, NiceStreamer falls back to the system default.

## Menu bar and shortcuts

- **Menu bar icon.** Shows your goal progress and lets you connect or disconnect, mute sounds or speech, skip speech, reset the counter, and open the main window or goal window.
- **Hide the Dock icon.** NiceStreamer then lives only in the menu bar.
- Closing the window never quits NiceStreamer, so sounds and the overlay keep working. Quit from the menu bar or with ⌘Q.
- **Shortcuts.** Set system-wide shortcuts for skipping speech, muting sounds, muting speech and resetting the counter on **Settings**. They work while another app is in front and each needs at least one modifier key.

## Profiles

On **Settings**, **Export profile** saves your sounds, commands, announcements, voice and goal settings as a single file, and **Import profile** loads one. It's handy for switching between setups or sharing a sound pack. A profile never includes your username, API key, chat login, shortcuts or connection settings.

## TikTok LIVE Studio

TikTok LIVE Studio has no public API, so NiceStreamer can't plug into it directly. Instead it reads your live room by username, which works whichever software you stream from. On the **Connection** page you can turn on **Connect when LIVE Studio opens**, which connects and disconnects automatically when an app named "TikTok LIVE Studio" is running on the same Mac.

## Updates

NiceStreamer checks for new versions on [GitHub](https://github.com/NiceSky-Studios/NiceStreamer/releases) when it opens (at most every few hours).

- When a new version is out, a banner appears on the **Dashboard** and the sidebar shows an **Update** link.
- Press **Update now** to download it, replace the app and reopen it. NiceStreamer never updates or restarts by itself, because that would cut your sounds and overlay mid-stream.
- The **About** page shows your version, a **Check now** button, the release notes for the new version, and a switch to turn automatic checking off.
- **Skip this version** hides the banner for that version until the next one comes out.
- If NiceStreamer can't replace itself where it is installed, it downloads the new version and shows it in Finder so you can drag it into your Applications folder.

## Troubleshooting

| Problem | Try this |
| --- | --- |
| Won't connect | Add an Euler Stream API key (Dashboard banner or Connection page), make sure you're live, and check the username |
| Says "authenticate_websocket is true, but no whitelist host defined" | Update to NiceStreamer 1.3.3 or newer. As a quick workaround, turn off Post replies in chat on the Connection page to connect without your login |
| NiceStreamer freezes or doesn't respond when opened | Update to 1.3.2 or newer. Older versions could freeze at launch while looking for cloudflared or Node.js. As a quick workaround, turn off the public https link before quitting |
| Says a request "requires a Business plan" | Update to NiceStreamer 1.3.1 or newer. NiceStreamer no longer asks for the feature that needed that plan. If it still appears, check your plan at eulerstream.com/pricing |
| "Node.js not found" | Install Node.js, or enter its path on the Connection page |
| Streaming app rejects the overlay link | Use the public https link or the goal window |
| "cloudflared isn't installed" | Run `brew install cloudflared`, then switch the public link off and on |
| Overlay link doesn't load | Check the status on Goal & Overlay; if the port is busy, pick another one |
| No sound on stream | Choose your loopback device under Settings, Audio output |
| Replies don't appear in chat | Check that Post replies in chat is on, your login is saved, your API key is set, and Post in chat is on for that command. Reconnect after changing login settings |
| A shortcut does nothing | It may already be used by macOS or another app. Pick a different combination |
| Speech never plays | Check the Voice page and that Text-to-speech is on in Quick controls |
| Google voice doesn't work | Check that the Text-to-Speech API is enabled for your key, that billing is set up if Google asks for it, and read the message in the activity feed. NiceStreamer speaks with your Mac voice meanwhile |

## Where your data lives

Settings, imported sound files and stream recaps are stored in `~/Library/Application Support/NiceStreamer`.

## Good to know

- The connection to TikTok is **unofficial**. It can stop working when TikTok changes things, until the underlying library is updated.
- Viewer counts, join events and diamond values come from the same unofficial connection, so they can occasionally be missing or late. Gift names and diamond values come straight from TikTok's gift messages, so an unusual gift can show up as just "gift" with no diamond value.
- NiceStreamer is an independent project and is not affiliated with or endorsed by TikTok, ByteDance or Euler Stream.
