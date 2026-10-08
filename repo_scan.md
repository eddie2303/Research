# AutonoMotion: open-source repo scan

Scan date: 2026-10-08. Star counts, last-push dates, licenses and open-issue counts come from GitHub repository search results fetched on that date. "Last commit" means the repo's `pushed_at` value, i.e. the last push to any branch. It is close to the last-commit date but not identical. Repos with no push since 2025-10-08 count as stale.

## 1. Verdict

No project comes close to AutonoMotion as a whole. Every end-to-end "YouTube automation" repo found is a faceless-content pipeline that uses TTS narration, and none of them makes script numbers traceable to sources. Two exceptions come nearer than the rest. `MoneyPrinterTurbo` accepts a user audio file, and the 0-star `Be-zerk/youtube-shorts-pipeline` fact-checks scripts against cited claims. Neither one combines source-traced numbers with human narration. The useful matches are pieces:
- a research pipeline with a claims ledger and number provenance (`deepdive`)
- a deterministic "no unsupported number in the prose" gate (`claimcheck`)
- a human-voice-first editor that puts Remotion graphics on word timings (`claude-youtube-editor`)

The stages for fixed-segment Shorts cutting and Google Drive asset organization have no good match. Every clipper uses an LLM to pick "viral" highlights, and Drive tooling is either a plain API wrapper or a Shorts uploader that writes metadata with AI. AutonoMotion's design (a human voice, a citation-carrying report, numbers gated before graphics) is less common than the faceless-channel genre, so expect to keep building the core yourself and borrow the parts listed below.

## 2. Candidates by pipeline stage

Verdict key: **Reuse** = take code or a dependency. **Study** = copy the design, not the code. **Skip** = not worth the time.

### Stage 1: Research (LLM web research with per-fact source URLs)

| Repo | Stars | Last commit | License | What it covers | Conflicts with constraints? | Verdict |
|---|---|---|---|---|---|---|
| [Socialpranker/deepdive](https://github.com/Socialpranker/deepdive) | 371 | 2026-10-04 | MIT | Claude Code research skill. Writes `claims.csv` (the claim ledger), `numbers.csv`, and one `sources/NN.md` file of verbatim quotes per source. Four-layer citation verification: liveness, faithfulness, qualifier preservation, construct provenance. | No. | **Study / Reuse scripts.** Best fit for the "every fact carries a URL" report. |
| [assafelovic/gpt-researcher](https://github.com/assafelovic/gpt-researcher) | 29,953 | 2026-10-01 | Apache-2.0 | Mature autonomous research agent (`GPTResearcher.conduct_research()` / `write_report()`), Tavily and MCP retrievers, cited reports. | No. The README does not show per-fact citation binding; output is a Markdown, PDF or Word report, not structured JSON. | **Study.** Possible Perplexity replacement, but you would need to add structured output yourself. |
| [stanford-oval/storm](https://github.com/stanford-oval/storm) | 31,592 | 2025-09-30 (stale by 8 days) | MIT | Research, outline, then a full article with citations. | No. | **Reference only.** Outside the 12-month window. Worth reading for how it ties citations to sentences, not for adoption. |
| [langchain-ai/open_deep_research](https://github.com/langchain-ai/open_deep_research) | 12,682 | 2026-08-10, **archived** | MIT | LangGraph deep-research reference implementation. | No. | **Skip.** Archived. |
| [tarun7r/deep-research-agent](https://github.com/tarun7r/deep-research-agent) | 188 | 2026-04-04 | MIT | LangGraph multi-agent research with citations and source credibility scoring. | No. | Skip. Small, and overlaps with the two above. |

### Stage 2: Parsing, validation and claim checking

| Repo | Stars | Last commit | License | What it covers | Conflicts? | Verdict |
|---|---|---|---|---|---|---|
| [FrancyJGLisboa/claimcheck](https://github.com/FrancyJGLisboa/claimcheck) | 0 | 2026-08-15 | MIT | Deterministic, stdlib-only, no LLM. Flags any number in prose that matches no value in an evidence JSON (±15% tolerance by default), fabricated quotes and out-of-window dates. CLI, library and GitHub Action. | No. | **Reuse the idea (and maybe the code).** It maps almost exactly onto "every number must trace to the report". Zero stars and two months old, so read all of it before depending on it. |
| [google-deepmind/long-form-factuality](https://github.com/google-deepmind/long-form-factuality) | 692 | 2026-06-18 | Other (NOASSERTION) | The SAFE method: split text into atomic facts and verify each one with search. | No. | Study. A research benchmark, not a library. |
| [Liyan06/MiniCheck](https://github.com/Liyan06/MiniCheck) | 228 | 2025-08-27 (stale) | Apache-2.0 | Small models that check whether a sentence is supported by a grounding document. | No. | Reference only. Could serve as an optional semantic check after `claimcheck`. |
| [Libr-AI/OpenFactVerification](https://github.com/Libr-AI/OpenFactVerification) (Loki) | 1,158 | 2024-10-03 (stale) | MIT | Claim decomposition, then evidence retrieval and verification. | No. | Skip. Stale. |
| [GAIR-NLP/factool](https://github.com/GAIR-NLP/factool) | 936 | 2024-08-19 (stale) | Apache-2.0 | Detects factual errors in LLM output. | No. | Skip. Stale. |
| [jamditis/claude-skills-journalism](https://github.com/jamditis/claude-skills-journalism) | 416 | 2026-10-04 | MIT | Newsroom skills: `fact-check-workflow`, `source-verification`, and hooks `source-attribution-check` and `data-methodology-check`. | No. | Study. Prompt material and editorial checklists, no number-verification code. |

Story severity scoring has no open-source match. None of the repos found does anything comparable.

### Stage 3: Script generation (fixed segments, numbers traced to the report)

| Repo | Stars | Last commit | License | What it covers | Conflicts? | Verdict |
|---|---|---|---|---|---|---|
| [Be-zerk/youtube-shorts-pipeline](https://github.com/Be-zerk/youtube-shorts-pipeline) | 0 | 2026-09-03 | None stated | Research keeps only Wikipedia sentences that carry a `<ref>`, resolved to URLs (`research_topic_free.py`). `fact_check_script.py` re-checks each script sentence against those facts. `validate_script.py` runs deterministic checks. | **Yes.** Kokoro TTS narration is built in, and visuals are AI images. | **Study only** `fact_check_script.py` and `validate_script.py`. With no license, the code is not legally reusable. |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | 129,171 | 2026-10-08 | MIT | Topic, then LLM script, stock footage, subtitles, video. Accepts a custom script and a `--custom-audio-file`. | Partly. TTS by default and scripts have no sources, but both can be overridden. | Skip as a base; it is built for stock-footage faceless Shorts. |
| [cskillzmartin/script-bot](https://github.com/cskillzmartin/script-bot) | 1 | not verified (updated 2025-10-20) | not verified | YouTube script generator with research, fact-checking and APA citations, all local. | Unknown. | Skip. Too small to rely on; listed because it matches the concept. |

Script generation has no good match. Nothing found produces a fixed segment structure or enforces number traceability. Build it in-house and gate it with a `claimcheck`-style check.

### Stage 4: Data graphics (stat, versus and quote cards, animated)

| Repo | Stars | Last commit | License | What it covers | Conflicts? | Verdict |
|---|---|---|---|---|---|---|
| [hassancs91/claude-youtube-editor](https://github.com/hassancs91/claude-youtube-editor) | 324 | 2026-08-18 | MIT | You record yourself; the tool keeps your voice (`/clean-audio` only isolates and levels it). Every full-screen statement or title card is Remotion TSX, with reveals synced to word timings. `tools/yt_upload.py` uploads a private draft. | No for voice, since it keeps the human recording. Thumbnails use Gemini with face reference photos, which you can skip. | **Study / Reuse.** The closest workflow to AutonoMotion's narration-plus-graphics stage. |
| [Vincentwei1021/video-talkcraft](https://github.com/Vincentwei1021/video-talkcraft) | 1,425 | 2026-10-01 | **PolyForm Noncommercial 1.0.0** | Voiceover-driven Remotion explainers. Takes "any TTS or human recording". `scripts/timestamps_cpu.py` aligns the script to the audio. 108 TSX recipe cards, including data shots and a number-roll component. | Voice: no. **License: yes.** A monetized channel counts as commercial use and needs the author's permission. | Study the design only. Do not copy code without a commercial grant. |
| [remotion-dev/remotion](https://github.com/remotion-dev/remotion) | 62,516 | 2026-10-08 | Remotion License | React-based programmatic video. Underlies both repos above. | Free for individuals and for-profit companies with up to 3 employees; larger companies need a Company License. | **Reuse** if you accept the TypeScript and Node stack. |
| [ManimCommunity/manim](https://github.com/ManimCommunity/manim) | 41,355 | 2026-10-08 | MIT | Python animation engine. Animated numbers, bars and text. | No. | **Reuse** for a pure-Python graphics stage. Stat and versus cards are simple scenes. |
| [Zulko/moviepy](https://github.com/Zulko/moviepy) | 14,960 | 2026-08-26 | MIT | Python compositing: overlay rendered card PNGs and animate position and opacity. | No. | Reuse. The simplest path from PNG cards to animated overlays. |
| [nexu-io/html-video](https://github.com/nexu-io/html-video) | 4,654 | 2026-06-21 | Apache-2.0 | HTML, CSS and data turned into MP4, with 21 templates and pluggable renderers. | No (the AI soundtrack is optional). | Study. An Apache-licensed alternative to Remotion if you want HTML cards. |
| [motion-canvas/motion-canvas](https://github.com/motion-canvas/motion-canvas) | 19,255 | 2026-07-02 | MIT | TypeScript code-driven animation. | No. | Skip. Overlaps with Remotion and Manim. |

### Stage 5: Narration (human only)

No repo is needed here, and this is the stage most projects break. Repos that work with a human recording: `claude-youtube-editor` (built around it), `video-talkcraft` (accepts it), `MoneyPrinterTurbo` (`--custom-audio-file`), and `Alexander-Kz/video-layer-skill` (takes a voiceover MP3, but the README does not say whether it must be human; 6 stars, last commit 2026-06-07, MIT, AI-generated whiteboard images). The useful technique is word-level alignment of your recording to the script (faster-whisper or a CTC aligner), which drives graphic timing.

### Stage 6: Shorts (3 per video, cut from fixed segments)

| Repo | Stars | Last commit | License | What it covers | Conflicts? | Verdict |
|---|---|---|---|---|---|---|
| [Anil-matcha/AI-Youtube-Shorts-Generator](https://github.com/Anil-matcha/AI-Youtube-Shorts-Generator) | 5,278 | 2026-10-06 | MIT | Long video to 9:16 shorts. Whisper transcript, LLM highlight ranking, ffmpeg plus OpenCV face-tracked crop (`shorts_generator/local/clipper.py`). Keeps the source audio. | No. | Study `local/clipper.py` only. The README lists no manual-timestamp mode, so highlight picking is LLM-driven, which AutonoMotion does not need. |
| [ClipsAI/clipsai](https://github.com/ClipsAI/clipsai) | 545 | 2024-01-17 (stale) | MIT | Python library for transcript-based clip finding and resizing. | No. | Reference only. Stale. |

Shorts cutting has no good match. Because the segments are fixed (cold open, reframe, take), the job is ffmpeg cuts at script-segment timestamps plus a vertical re-layout of the graphics. Every clipper found is built around LLM "virality" detection and face tracking, which a presenter-less channel does not need.

### Stage 7: Publishing and Google Drive asset organization

| Repo | Stars | Last commit | License | What it covers | Conflicts? | Verdict |
|---|---|---|---|---|---|---|
| [iterative/PyDrive2](https://github.com/iterative/PyDrive2) | 668 | 2026-08-05 | Other (NOASSERTION) | Maintained Python wrapper for the Google Drive API. | No. | Reuse if you prefer a wrapper; the official `google-api-python-client` also works. |
| [SJbuilds04/yt-auto-uploader](https://github.com/SJbuilds04/yt-auto-uploader) | 24 | 2026-06-23 | None stated | Pulls videos from Drive, writes titles, descriptions and tags with AI, schedules uploads to YouTube. | Risk: the AI-written descriptions could contain unsourced numbers. | Study the Drive-to-YouTube flow only. No license. |
| [dineshbarri/AI-Video-Factory-Veo3-Automation-Pipeline](https://github.com/dineshbarri/AI-Video-Factory-Veo3-Automation-Pipeline) | 26 | not verified | not verified | n8n workflow covering Veo3 generation, Drive storage, and YouTube upload. | Yes. AI-generated video. | Skip. |

Drive asset organization has no good match. Nothing found organizes per-episode folders (script, report, cards, narration takes, Shorts). Write that yourself; the API work is small.

### End-to-end "YouTube automation" projects (checked, not recommended)

| Repo | Stars | Last commit | License | Why not |
|---|---|---|---|---|
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | 129,171 | 2026-10-08 | MIT | Faceless stock-footage Shorts; no sourcing (see Stage 3). |
| [RayVentura/ShortGPT](https://github.com/RayVentura/ShortGPT) | 8,009 | 2025-02-10 (stale) | MIT | Stale; TTS-centric Shorts automation. |
| [digitalsamba/claude-code-video-toolkit](https://github.com/digitalsamba/claude-code-video-toolkit) | 2,180 | 2026-10-05 | MIT | Documented around ElevenLabs and Qwen TTS, `/voice-clone`, and `tools/redub.py`. Importing your own audio is not described. Its Remotion templates and `youtube_upload` tool are worth a look. |
| [raunakpatil/youtube-agentic-ai-studio](https://github.com/raunakpatil/youtube-agentic-ai-studio) | 96 | 2026-06-03 | MIT | Gemini "brainstorms" topics with no web sources; Edge-TTS narration. |
| [hueanmy/ai-shorts-generator](https://github.com/hueanmy/ai-shorts-generator) | 28 | 2026-04-26 | None stated | "Data-driven tech news" topic, but ElevenLabs voice-over; no license. |

## 3. Top 3 to borrow from

1. **Socialpranker/deepdive** (research and validation, Stages 1–2). Take the artifact design:
   - `claims.csv` (claim ledger with triangulation status)
   - `numbers.csv` (every figure the report relies on)
   - one `sources/NN.md` file per source, holding verbatim quotes
   - a `caveat:` field limited to `vendor`, `self-reported` or `disputed:sNN`, which caps confidence
   - the number checks `check_number_provenance.py` (does one value circulate across "independent" sources?) and `check_number_arithmetic.py` (recompute derived figures and shares)
   - `eval/check_citations.py` (checks that URLs still resolve)

   Restructuring the Perplexity output into `numbers.csv` plus `sources/` turns "every number traces to the report" into a lookup instead of a judgment call. The license is MIT. Most of the methodology is prompt Markdown (`SKILL.md`, `references/`), so port the schemas and scripts, not the agent.

2. **FrancyJGLisboa/claimcheck** (script gate, Stage 3, and an input check for Stage 4). Use `check(prose, data)` or the CLI `claimcheck --prose script.md --data report_numbers.json --strict` as a blocking step after script generation and before card rendering: any number in the script that is absent from the report fails the build. It is stdlib-only and MIT. Raise the precision by tightening `--tolerance` (the default is ±15%, which is loose for stat cards). Caveat: it has 0 stars and one author, so vendor a reviewed copy rather than depending on it. Optional upgrade: an LLM or MiniCheck pass for semantic contradictions, which `claimcheck` explicitly does not catch.

3. **hassancs91/claude-youtube-editor** (graphics and narration integration, Stages 4–5 and part of 7). Take the pattern of a recorded human voice, then a transcript with word timings, then Remotion shots (`remotion/src/shots/<project>/`, shared kit in `remotion/src/lib/`) whose reveals are keyed to word timings. Also take the `brand.md` style contract and `tools/yt_upload.py`, which checks for ghost speech and audio/video drift before uploading a private draft. The license is MIT, but Remotion's own license applies. If you would rather stay in Python, apply the same timing approach to Manim scenes or moviepy overlays. Skip the Gemini face-thumbnail step.

Runner-up: `video-talkcraft`'s card catalogue (`template/cards/`, `references/taxonomy.md`) and its CPU word-alignment script are good design references. Its PolyForm Noncommercial license rules out copying code for a monetized channel without the author's permission.

## 4. Hard-constraint violations, plainly

- **Forced or default TTS (violates human-narration-only):**
  - `Be-zerk/youtube-shorts-pipeline`: Kokoro TTS; the README describes no human-audio path.
  - `raunakpatil/youtube-agentic-ai-studio`: Edge-TTS default; ElevenLabs cloning on its roadmap.
  - `RayVentura/ShortGPT` (also stale).
  - `hueanmy/ai-shorts-generator`: ElevenLabs.
  - `Vincentwei1021/anything2explainer` (2,338 stars, TTS voiceover by design).
  - `indiser/ViralContent-Factory` (Edge-TTS).
  - Most "faceless" repos found, for example `Dark2C/Viral-Faceless-Shorts-Generator` and `mzu-2410z/yt-automation`.
  - `MoneyPrinterTurbo` defaults to TTS but accepts `--custom-audio-file`, so it can be used safely.
- **AI voice cloning or redubbing:** `digitalsamba/claude-code-video-toolkit` (`/voice-clone`, `tools/redub.py`). Do not adopt those features.
- **AI avatars or presenters:**
  - `safant-13/AI-SHORTS-GENRATOR-newsFlash-`: D-ID avatars, per its description.
  - `Awaisali36/ai-avatar-video-generation-system`: AI-avatar news videos, per its description.
  - `claude-youtube-editor` uses Gemini face renders for thumbnails only; drop that step.
- **Unsourced LLM numbers:**
  - Every end-to-end generator above writes scripts from an LLM with no source binding: MoneyPrinterTurbo, ShortGPT, youtube-agentic-ai-studio, `darkzOGx/youtube-automation-agent` (4,134 stars; README not reviewed beyond its description).
  - `SJbuilds04/yt-auto-uploader` writes video descriptions with AI, so unsourced figures could reach published metadata.
- **License conflict (not a stated constraint, but blocking):** `video-talkcraft` is PolyForm Noncommercial. `Be-zerk/youtube-shorts-pipeline`, `SJbuilds04/yt-auto-uploader` and `hueanmy/ai-shorts-generator` state no license, which means no reuse rights.

## Method

- **Date:** 2026-10-08.
- **Tools:**
  - `gh` CLI unavailable: `gh auth status` reported that the `GH_TOKEN` token is invalid.
  - Direct `api.github.com` calls were refused by this session's policy, which only allows repository-scoped endpoints for attached repos.
  - Used instead: GitHub MCP tools (`search_repositories` for discovery and metadata, `search_code` for code search) and WebFetch of `raw.githubusercontent.com` READMEs.
  - Nothing was cloned or installed.
- **Repository queries:**
  - "automated youtube video pipeline"
  - "faceless youtube automation"
  - "news to video llm"
  - "news video generator"
  - "deep research agent citations"
  - "claim verification source grounding llm"
  - "long video to shorts clipper"
  - "programmatic motion graphics python"
  - "data card generator video"
  - "voiceover explainer video"
  - "youtube script research citations"
  - "google drive youtube upload automation"
  - topic searches: `topic:youtube-automation`, `topic:deep-research`, `topic:fact-checking`, `topic:opus-clip-alternative`, `topic:motion-graphics`, `topic:programmatic-video`
  - star-filtered variants, several of which returned nothing: "shorts clips long video stars:>500", "research report citations web search agent stars:>2000", "fact verification llm claims evidence stars:>200"
- **Code search queries:**
  - `"sonar" perplexity citations youtube script language:python`
  - `"stat card" "quote card" video language:python`
  - `"cold open" segments script youtube shorts language:python`

  These returned only tiny or unrelated repos; none made the tables.
- **Zero-result queries:**
  - "animated infographic statistics video generator"
  - "quote card image generator python pillow"
  - "explainer video pipeline own voiceover"
  - "documentary youtube script research sources llm"
  - "news explainer video automation"
  - "google drive video upload organize youtube pipeline"
  - "youtube video generator llm script stars:>500"
- **Metadata:** stars, `pushed_at`, license and open issues were fetched in batches (`repo:a/b repo:c/d …`). Rows marked "not verified" lacked a full metadata fetch.
- **Voiceover checks** were made from READMEs and are only as reliable as each README.
- **Caveat on stars:** several 2026 repos (e.g. `video-shotcraft`, `anything2explainer`, `video-talkcraft`) have high star counts for their age. The counts are reported as GitHub shows them; treat them as weak evidence of quality.
