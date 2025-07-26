# Youtube-Summarizer
# YouTube Subtitle Summarizer

> **Summarize any YouTube video that already contains subtitles (automatic or creator‑uploaded) into a concise, readable paragraph or bullet list in seconds.**

---

## ⭐️ Features

| Capability                      | Details                                                                                                                                                      |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Subtitle extraction**         | Uses the latest [`youtube‑transcript‑api`](https://pypi.org/project/youtube-transcript-api/) (`fetch()` syntax) to pull captions in your preferred language. |
| **Smart chunking**              | Breaks long transcripts on sentence boundaries so the summarizer never exceeds model token limits.                                                           |
| **State‑of‑the‑art summarizer** | Defaults to `facebook/bart‑large‑cnn` via 🤗 Transformers; easily swappable for GPT‑4o or any other LLM.                                                     |
| **Multi‑URL support**           | Accepts full, short (`youtu.be/`), *Shorts*, and *embed* links—extracting the video ID automatically.                                                        |
| **CLI & importable module**     | Run `python summarizer.py <YouTube‑URL>` **or** call `summarize_youtube(url)` from your own code.                                                            |
| **Extensible**                  | Add sentiment analysis, keyword extraction, or database storage of transcripts with only a few lines of code.                                                |

---

## 🏗  Architecture

```text
┌────────────────┐   1. URL → Video‑ID   ┌────────────────┐   2. Fetch   ┌────────────────┐
│  YouTube Link  │──▶ extractor.py  ───▶│ youtube-trans. │──▶ captions ─▶│   chunker.py   │
└────────────────┘                      └────────────────┘              │                │
        ▲                                                              │   3. Split     │
        │                                                              └────────────────┘
        │                                                                      │
        │                                            4. Summarize chunks       ▼
┌────────────────┐                                                     ┌────────────────┐
│    CLI /      │◀─────────────────────────────────────────────────────│ summarizer.py  │
│ other scripts │                 final summary                        └────────────────┘
└────────────────┘
```

---

## 📦  Requirements

* Python **3.9+**
* [youtube‑transcript‑api](https://pypi.org/project/youtube-transcript-api/) ≥ **1.2.1**
* [transformers](https://pypi.org/project/transformers/) ≥ **4.41**
* [torch](https://pytorch.org/) — CPU is fine, but GPU (CUDA) speeds up large models.

> **Optional:** for development, install `pre‑commit` so black/flake8 run automatically.

---

## 🚀  Quick Start

```bash
# 1 . Clone the repo
$ git clone https://github.com/your‑handle/youtube‑subtitle‑summarizer.git
$ cd youtube‑subtitle‑summarizer

# 2 . Create & activate a virtual environment (recommended)
$ python -m venv .venv
$ source .venv/bin/activate   # Windows: .venv\Scripts\activate

# 3 . Install dependencies
$ pip install -r requirements.txt

# 4 . Run!
$ python summarizer.py https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

---

## 🖥  Usage

### CLI

```bash
python summarizer.py <YouTube‑URL>       # prints summary to stdout
python summarizer.py -l es <URL>         # try Spanish captions first
python summarizer.py -o summary.txt <URL># write summary to a file
```

### As a library

```python
from summarizer import summarize_youtube

summary = summarize_youtube("https://youtu.be/dQw4w9WgXcQ")
print(summary)
```

---

## ⚙️  Configuration

| Flag / variable           | Description                                                   | Default                   |
| ------------------------- | ------------------------------------------------------------- | ------------------------- |
| `--languages, -l`         | Comma‑separated caption language priority list (e.g. `en,hi`) | `en,en-US`                |
| `--model`                 | HuggingFace model name or alias                               | `facebook/bart-large-cnn` |
| `--chunk-size`            | Max characters per transcript chunk                           | `3800`                    |
| `--min-len` / `--max-len` | Token limits for each summary call                            | `30` / `150`              |

You can also export environment variables (e.g. `SUMMARY_MODEL`, `CHUNK_SIZE`) for Docker / cloud runs.

---

## 🧩  Extending

* **Different summarizer** — swap the model name or call an OpenAI API wrapper.
* **Vector DB / RAG** — store full transcripts in Pinecone, Supabase, etc. for Q\&A.
* **Streamlit / Gradio UI** — wrap `summarize_youtube` in a simple web app.
* **Batch mode** — feed a playlist or channel URL, summarising every video to Markdown files.

---

## 🐞  Troubleshooting

| Problem                        | Cause                                                   | Fix                                                     |
| ------------------------------ | ------------------------------------------------------- | ------------------------------------------------------- |
| `Transcript fetch failed: ...` | Video has no captions / age‑restricted / region‑blocked | Provide your own transcript or use Whisper ASR version. |
| Empty / poor summary           | Very short captions or music‑only video                 | That’s expected; try longer content or ASR pipeline.    |
| CUDA OOM                       | GPU model too big                                       | Use `sshleifer/distilbart-cnn-12-6` or run on CPU.      |

---

## 🗺  Roadmap

* [ ] Add Whisper‑ASR fallback for captionless videos
* [ ] Live URL preview UI (Streamlit)
* [ ] Export summaries to Notion / Obsidian automatically

---

## 🤝  Contributing

1. Fork the repo & create your branch (`git checkout -b feature/foo`)
2. Commit your changes (`git commit -m 'Add foo'`)
3. Push to the branch (`git push origin feature/foo`)
4. Open a Pull Request

All code must pass **black**, **isort** and **flake8**.

---

## 📝  License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏  Acknowledgements

* The authors of **youtube‑transcript‑api**
* Facebook AI for **BART**
* Hugging Face team & community
