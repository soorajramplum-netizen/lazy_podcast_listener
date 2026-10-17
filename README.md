# lazy_podcast_listener 🛋️🎧

**Too long; didn't listen.** A free, local tool that turns YouTube podcasts into clean, readable Markdown transcripts, so you can read, summarise, and keep every idea without watching hours of video.

<!-- Add a screenshot: save it as docs/screenshot.png in this repository, then delete this line and the one below.
![Screenshot of lazy_podcast_listener](docs/screenshot.png) -->

---

## Why I built this

I love podcasts. The problem is that my list of episodes kept growing and my free time didn't.

Every week there were conversations I genuinely wanted to hear: founders, researchers, operators sharing things that took them years to learn. Most were two or three hours long. I'd save them for "later," later never came, and the ideas I would have valued were simply gone.

Summaries from other people didn't fix it. Someone else was deciding what mattered, and I never knew what had been left out.

So I built **lazy_podcast_listener**. I give it a list of YouTube links, and it gives me back the complete transcript of every episode as a clean Markdown file. Then I:

- **Summarise on my own terms.** I give the full transcript to an AI model and ask the questions *I* care about. Because the summary comes from everything that was said, I know I'm not missing anything important.
- **Jump back to the good parts.** Every paragraph has a clickable timestamp. When something in a summary catches my attention, one click takes me to that exact moment in the episode.
- **Build a library of missed learnings.** Every episode becomes a searchable file. Over time, this has turned into a personal knowledge base of ideas I would otherwise have lost, and it drops straight into Obsidian or Notion.

Lazy? Maybe. But I haven't missed a good idea since.

---

## What it does

- **Bulk input.** Paste links or upload a CSV of episodes. Links can be in any column; standard links, `youtu.be` links, live streams, and bare video IDs all work, and duplicates are removed.
- **Checks before downloading.** A review step tells you upfront what's available, e.g. *"21 of 25 videos can be transcribed,"* with the reasons for any that can't.
- **Readable transcripts.** Raw captions are grouped into proper paragraphs instead of a wall of three-word lines.
- **Clickable timestamps.** Each paragraph links to that moment in the video. Timestamps can also go on every line, or be turned off.
- **Language control.** Pick preferred languages in order (e.g. `en, ta, hi`), fall back to whatever language an episode has, or machine-translate it into your first choice.
- **Knowledge-base friendly.** Optional YAML front matter (title, channel, date, language) for Obsidian, Notion imports, and static-site tools.
- **One download.** Everything comes back in a single ZIP, with an index file linking every transcript.
- **Works without captions, too.** For episodes with no captions, optional [Whisper](https://github.com/SYSTRAN/faster-whisper) transcribes the audio on your own computer, free.
- **Private and free.** Runs entirely on your machine. No accounts, API keys, or subscriptions.

---

## Getting started

### Windows

1. Install [Python](https://www.python.org/downloads/) 3.9 or newer. On the installer's first screen, tick **"Add python.exe to PATH."**
2. Download this repository: click the green **Code** button, then **Download ZIP**.
3. Right-click the ZIP, choose **Extract All**, and open the extracted folder.
4. Double-click **`start.bat`**. If Windows shows a security prompt, click **More info**, then **Run anyway**.

The first launch installs what it needs, which takes a minute or two, and then the app opens in your browser. After that, double-click `start.bat` any time and it starts in seconds. Keep the black window open while you use the app; close it when you're done.

### Mac / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

The app opens at `http://127.0.0.1:8765`.

### Optional: Whisper for episodes without captions

- **Windows:** double-click `install-whisper.bat`, then restart the app with `start.bat`.
- **Mac / Linux:** run `pip install -r requirements-whisper.txt` and install [Deno](https://deno.com/) (`brew install deno`).

The first transcription downloads the speech model (about 0.5 GB). After that it runs offline. Speed depends on your computer; an NVIDIA GPU is used automatically if you have one.

---

## How to use it

1. Choose **Transcripts**.
2. **Add episodes:** upload a CSV or paste YouTube links. A CSV is checked automatically.
3. **Review** what's available. Adjust languages here if you need to; the count updates instantly.
4. Click **Generate**, then **Download ZIP**.

### What you get

```
transcripts/
  Episode title [videoID].md   one file per episode
_index.md                      links to every transcript
_skipped.csv                   anything that couldn't be transcribed, and why
```

Each transcript looks like this:

```markdown
# How Great Companies Are Built

- **Channel:** Example Podcast
- **Video:** https://www.youtube.com/watch?v=VIDEO_ID
- **Transcript:** English (auto-generated)
- **Retrieved:** 2026-10-17

---

## Transcript

[00:00](https://www.youtube.com/watch?v=VIDEO_ID&t=0s) Welcome back to the show. Today we're talking about...

[00:34](https://www.youtube.com/watch?v=VIDEO_ID&t=34s) The first thing most founders get wrong is...
```

---

## How I use the transcripts

### The 5-minute version of a 3-hour episode

I upload the transcript to an AI assistant with a prompt like:

> Summarise this episode in 10 key ideas. For each one, include the timestamp where it's discussed and one sentence on why it matters. Then list any actionable advice, and every book, tool, or person mentioned.

### Asking my own questions

> What does the guest believe that most people in their industry would disagree with? Quote them directly and include timestamps.

> I'm working on [your project]. Which parts of this conversation are most relevant to me, and why?

### Building the library

I keep every transcript in one folder (an Obsidian vault works well), with YAML front matter turned on. That makes it possible to search across months of episodes, link related ideas between guests, and ask an AI to find patterns across many conversations at once.

---

## Troubleshooting

- **"YouTube is temporarily limiting requests":** too many requests in a short time. Wait 5 to 15 minutes, then click **Retry failed**. The app already retries automatically.
- **Everything suddenly fails:** YouTube may have changed something. Delete `.venv\installed-v2.txt` and run `start.bat` again to update the libraries. If that doesn't help, delete the whole `.venv` folder for a fresh setup.
- **"Port 8765 is busy":** the app is already running in another window. Close it first.

---

## Project structure

```
start.bat                  Windows: double-click to set up and launch
install-whisper.bat        Windows: optional Whisper setup
app.py                     Local web server
core.py                    Checks and fetches YouTube captions
whisper_engine.py          Optional local transcription
public/index.html          The interface
public/sample.csv          Example input
requirements.txt           Required packages
requirements-whisper.txt   Optional Whisper packages
```

## Built with

- [youtube-transcript-api](https://github.com/jdepoix/youtube-transcript-api): fetching captions
- [yt-dlp](https://github.com/yt-dlp/yt-dlp): downloading audio for Whisper
- [faster-whisper](https://github.com/SYSTRAN/faster-whisper): optional offline transcription
- [PapaParse](https://www.papaparse.com/) and [JSZip](https://stuk.github.io/jszip/): CSV reading and ZIP creation in the browser

## Limitations

- Transcripts need captions (manual or auto-generated), unless Whisper is installed.
- Private, members-only, and age-restricted videos can't be accessed.
- The tool relies on YouTube's public pages, which change occasionally. Updating the libraries usually fixes things.

## Responsible use

Transcripts belong to the creators who made the content. This tool is meant for personal learning. Please respect [YouTube's Terms of Service](https://www.youtube.com/t/terms) and don't republish other people's work.

## License

Released under the [MIT License](LICENSE).
