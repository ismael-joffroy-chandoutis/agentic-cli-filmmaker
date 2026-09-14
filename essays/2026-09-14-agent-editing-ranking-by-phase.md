**English** · [Français](2026-09-14-agent-editing-ranking-by-phase.fr.md)

# Agent-driven editing: the ranking, phase by phase

*Field note, 14 September 2026, updated the same morning with two more tests. Companion to [Editing with agents, by type of work](2026-09-13-editing-with-agents-by-type-of-work.md). That text explains the position. This one is the tables: which tool for which phase, which machine, which channel, and what died. Written for someone who does not live in an editing suite. Everything marked "measured" was run on my own machines between 13 and 14 September; the rest is read from documentation and repositories the same day.*

## In three sentences

Nobody has one tool that makes a film end to end with an agent, and it is not close. What exists is a chain of formats and APIs around two closed hosts most editors already own: Final Cut to cut by hand, Resolve Studio for everything the agent does alone. The word that sorts everything is **headless**: does the tool render, cut or export without a window open. The "agent-first" surfaces of 2026 (Palmier, Velorn, Frontstage, NodeTool, basketikun) are all under six months old; test two or three for an hour each, never put the master there.

Lexicon: **base** = you can lean on it today · **lab** = interesting, worth an hour, never the source of truth · **forget** = dead, licence trap, or no real agent channel · **headless** = runs without a screen, can be launched by an agent or a cron on a GPU tower.

## 0. Phase by phase: what you do, where, and what the agent does

| Phase | What you do | Where you are | What the agent does, through which channel | Headless | My choice, in a few words |
|---|---|---|---|---|---|
| 1a. Collect references | Hunt, link, annotate | Are.na (open API); Freeform on iPad for the sofa wall; Kosmik set aside (no API) | Files, links, retrieves (Are.na API) | yes | Are.na: the API is alive |
| 1b. Think a scene on a wall, compare worlds | Place, compare, discard, keep; generate next to the source | A local shot-store board as the truth; basketikun infinite-canvas (MIT, MCP, canvas.best is its hosted version) as the lab; Martini for wall plus timeline plus 3D set, closed | Places media, generates variants linked to their source (ComfyUI on the towers); basketikun's MCP verified against its own store | no, it is for the eye; the server, yes | the board: named takes, ComfyUI; 1a and 1b share one surface once it imports Are.na |
| 1c. Wire a recipe that repeats | Once the look is locked: same character, twenty shots | OpenChar Studio (ex Inline, GPL, versioned takes, ComfyUI backend); Flora and Krea Nodes, closed; NodeTool (AGPL, official MCP) as a workshop | Re-runs the recipe, recomputes only downstream, keeps every take | yes | OpenChar: versioned takes, GPL; never a second graph tool to escape ComfyUI |
| 1d. Continuity of characters and sets | Decide what never changes | Visual bible on the board; Raccord (MIT, MCP) and OpenChar for drift measurement; LoRAs on the towers | Measures drift, keeps the bible, re-runs | yes | the bible on the board, Raccord measures |
| 2. Writing, breakdown | Write, break down, decide | Text: Obsidian, Notion; a home-made story curve | Transcribes, structures, proposes the curve, keeps the journal | yes | Obsidian alone for writing; Notion as a one-way mirror for the team |
| 3. Boards and animatic | Judge rhythm before paying for a generation | Blender VSE plus Grease Pencil, or Resolve; Storyboarder for its ergonomics only | Aligns boards on temp sound, renders the animatic (`bpy`, Resolve MCP) | yes (Blender in background) | Resolve when the boards are images (the animatic becomes the cut); Blender when the line or the camera matters (every angle already exists) |
| 4. Live shoot | Film what must be embodied | Blackmagic 6K, iPhone ProRes log | Ingest, LUT, transcription, shot register (Kitsu with a crew) | yes | ffmpeg, an indexer, Parakeet |
| 5. Generating images and shots | Choose the take that stays | The board (versioned takes) over ComfyUI on the towers; Krita AI for stills; Pallaidium to test | Generates, versions, writes the sidecar and the C2PA (ComfyUI API, MCP) | yes | ComfyUI on the towers through the board; Pallaidium in remote mode inside Blender (measured, Mac included); simple fronts over ComfyUI, never another graph |
| 6. Compositing and VFX | The eye on detail, shot by shot | Resolve Fusion (Studio); Blender compositor for 3D; Natron in the lab | Places a version at timecode, disables the old one (native MCP, verified) | app open | Fusion, the shot stays in the timeline; Blender for its own passes; Natron when it is stable; After Effects only when delivered |
| 7a. Cutting the film, documentary and narrative | Cut the meaning, with a Final Cut editor | Final Cut Pro | Prepares: selects, first assembly, markers, as FCPXML files | preparation yes, cut no | Final Cut: my cut, my editor |
| 7b. Cutting excerpts and social | Review, give the go | Palmier driven by a coding agent (measured), or ffmpeg plus a pipeline runner with no interface | Cuts, subtitles, reframes, exports FCPXML to Final Cut or Resolve (Palmier MCP, 51 tools) | Palmier: app open; ffmpeg: yes | Palmier, under a second per cut, then Final Cut without a dialog |
| 7c. Hybrid cut, VFX | Arbitrate generated shots | Resolve Studio, Mac and Windows | Everything it does alone on a timeline (native MCP) | app open | Resolve, the agent's host |
| 8. Sound | Listen, mix, or the mixer | REAPER (scriptable); Fairlight in Resolve | Transcribes (Parakeet), cleans, normalises, fades (Resolve MCP verified) | partial | REAPER for the mix, Fairlight for what lives in the timeline, ffmpeg and auto-editor headless; a full sound ranking follows |
| 9. Colour and conform | Grade, or the colourist | Resolve Studio; OpenColorIO; OTIO and FCPXML bridge | Imports structure, exports, relinks media (verified on Palmier's FCPXML) | export yes | Resolve plus OTIO, structure only |
| 10. Delivery | Give the go | Compressor; DCP-o-matic | Encodes, declines, subtitles (ffmpeg, HandBrake, whisper) | yes | ffmpeg, Compressor, DCP-o-matic |
| 11. Demos, motion, titles | Review, decide the graphics | Remotion, HyperFrames for the web; Apple Motion inside a film | Writes and renders (measured) | yes for the web | Remotion for components, HyperFrames and its local Studio for a page, Motion by cloning templates |

Two rules run through the table: between 7a and 7c the split is by sequence, never by shot; and the agent channel exists only on desktop machines, the iPad is for reviewing.

## 1. By situation: what do I open, who does what

| Situation | Tool to open | What the agent does | What you do | Headless |
|---|---|---|---|---|
| A 60-second excerpt for Instagram, TikTok, YouTube | Nothing: a pipeline runner or ffmpeg plus Parakeet, then review | Cuts, subtitles, reframes in three formats, exports | You review and give the go to publish | yes |
| A first assembly of a two-hour interview | Final Cut (or Palmier as a test) | Transcribes, proposes selects and an assembly as FCPXML | You cut the meaning in Final Cut | preparation yes, cut no |
| Generate a shot and place it in the film | ComfyUI on a tower, then Resolve Studio | Generates, places the version at timecode, disables the old one, writes the sidecar | You choose the take that stays | generation yes, placement yes if Resolve is open |
| A reference wall for a scene | A shot-store board, basketikun infinite-canvas as the lab | Places media, generates variants next to them | You compare, discard, keep | no, a surface for the eye |
| An animatic before paying for a generation | Blender VSE (boards plus temp sound) or Resolve | Aligns boards on sound, renders the animatic | You judge the rhythm | yes (Blender in background) |
| A demo of my tool or a GitHub repository video | Remotion or HyperFrames | Writes the script, the fake terminal, renders 16:9 and 9:16 | You review | yes |
| Titles, credits, graphics | Apple Motion inside a film; HyperFrames or Remotion for the web | Produces self-contained sequences you re-import | You decide the graphics | yes for the web, no for Motion |
| Conform, grade, deliver a DCP | Resolve Studio, OpenTimelineIO for exchange, DCP-o-matic | Imports the structure, exports the versions | You grade, or the colourist | export yes, grading no |

## 2. By phase: the ranking

Three criteria per tool, marked **yes / partial / no**: *works today* (mature, maintained) · *an agent drives it* (living API, MCP, CLI or readable project) · *exports to Final Cut or Resolve*. Plus the headless column.

### Ideation, references, wall

| Rank | Tool | Works | Agent | To FCP/Resolve | Headless | Verdict |
|---|---|---|---|---|---|---|
| 1 | A local shot-store board (home-made, ComfyUI backend) | partial | yes | through the store | no | base at home |
| 2 | basketikun/infinite-canvas (MIT, 6,470 stars, v0.18 on 7 Sept.) | yes | yes, MCP for coding agents | no, media plus JSON | no | priority lab |
| 3 | NodeTool (AGPL, 523 stars, official MCP, film surfaces) | yes | yes | no, MP4 | server yes | lab: a workshop for boards and generation, not a wall |
| 4 | BeatDesign (Apache, 27 stars, 29 MCP tools, 3 Sept.) | young | yes | no | no | lab |
| 5 | Excalidraw, AFFiNE (human walls, MCP) | yes | mostly read | no | no | wall without video |
| forget | Jaaz (non-OSI licence, quiet since March), hero8152 (stopped 28 Aug.), Boardfish (source-available, no API), Vibe Workflow, OpenFlow, Loomic (cloud) | | | | | |

### Boards, breakdown, animatic

| Rank | Tool | Works | Agent | To FCP/Resolve | Headless | Verdict |
|---|---|---|---|---|---|---|
| 1 | Blender 5.2 VSE plus Grease Pencil | yes | yes, `bpy`, 95 % of the app | video render, OTIO by add-on | yes | animatic base |
| 2 | Resolve (timeline of boards plus sound) | yes | yes, native MCP | it is Resolve | app open | base |
| 3 | Kitsu (shot register, REST) | yes | yes | metadata | yes | useful lab with a crew |
| 4 | OpenFrame (AGPL, 115 stars, FCPXML) | young | partial | yes | ? | lab |
| forget | Storyboarder (dead since 2022), Storyboard Tool (no licence) | | | | | |

### Generating images and shots

| Rank | Tool | Works | Agent | To FCP/Resolve | Headless | Verdict |
|---|---|---|---|---|---|---|
| 1 | ComfyUI 0.35 plus the official MCP (local and cloud) on the towers | yes | yes | files | yes | base |
| 2 | Pallaidium in Blender VSE (GPL, 1.5k stars, updated July) | yes, Windows mostly | yes, `bpy` plus MCP wrappers | VSE strips | yes | base for a 3D plus AI studio inside a VSE; installed on one tower, to test |
| 3 | Krita AI Diffusion (stills) | yes | partial | files | no | stills base |
| 4 | OpenChar Studio (ex Inline, GPL, local engine, LTX-2.5, FLUX.2) | active | JSON graph, no MCP | files | partial | lab, shot by shot |
| 5 | Raccord (MIT, local MCP, film continuity) | young | yes | OTIO/EDL announced | ? | lab |
| 6 | ComfyUI-SecondUnit (1 star, places media into Resolve) | too young | | yes | | watch |
| forget | artokun/comfyui-mcp (archived 10 Sept.), the Higgsfield Resolve plugin (cloud credits, no agent channel in the plugin) | | | | | |

### Compositing and VFX

| Rank | Tool | Works | Agent | Headless | Verdict |
|---|---|---|---|---|---|
| 1 | Fusion inside Resolve Studio | yes | yes, Python and Lua, native MCP | app open | production base (closed) |
| 2 | Blender Compositor | yes | yes, `bpy` | yes | base for 3D shots |
| 3 | Natron 2.5.1 preview | fragile | Python plus beta MCP | yes (NatronRenderer) | lab |

### Editing

| Rank | Tool | Works | Agent | To FCP/Resolve | Headless | Verdict |
|---|---|---|---|---|---|---|
| 1 | Resolve Studio 21.1 (native MCP, measured) | yes | yes | it is the host | app open | base for everything the agent does alone |
| 2 | Final Cut Pro 12.3 | yes | FCPXML files, plus SpliceKit (built from source with the two open pull requests, alive on 12.3: 222 MCP tools, 213 RPC methods, reads in 1 to 6 ms) as an analysis and finishing layer on an open library: local Parakeet transcription with diarisation, text-based editing, silences, captions, direct FCPXML export, OTIO, BRAW. No project creation by parameters, no placement with source bounds: those go through Final Cut's dialogs | it is the host | no | base for the hand cut; to build a cut by agent the route stays Palmier plus FCPXML import; SpliceKit serves once the library is open, and breaks at every Apple update |
| 3 | MLT / melt plus ffmpeg | yes | yes, XML plus CLI | through OTIO | yes | headless base for excerpts |
| 4 | Palmier (14,385 stars, MCP 51 tools, writes FCPXML 1.10, closed since 28 Aug., macOS 26) | yes, **measured**: 3 clips, 8 cuts, a title, a track change, export, re-import into Resolve on the exact frame, media relinked by itself | yes, the best addressing seen (`clipId`, frames) | yes, verified in Resolve; Final Cut import without a dialog | app open | **base for agent-driven excerpts**, never the master |
| 5 | Velorn (GPL, 475 stars, 100+ MCP tools, FCPXML, v0.3.33 on 10 Sept.) | 0.3 | yes | announced, not validated | ? | lab; MCP listening 4 s after launch (measured), test unfinished |
| 6 | Shotcut plus shotcut-mcp (GPL, MLT) | yes | active third-party MCP | through MLT/OTIO | melt yes | lab |
| 7 | Kdenlive (GPL, MLT XML, official OTIO adapter) | yes | MCP forks only | OTIO | no | alive, not obsolete, no living channel |
| 8 | Frontstage (GPL, port of Palmier, MCP 51 tools, FCPXML) | 10 stars, one week | yes | announced | web | a fork born a week ago, whatever a search engine calls it. Watch |
| 9 | Diffusion Studio (MPL, alpha, "the timeline is code") | alpha | skills plus CLI | no | yes | an engine to read, not a timeline |
| 10 | Timeline Studio (MIT, PWA, MCP as an extension) | v1.0.8 | yes | no | no | web lab |
| 11 | OpenShot 4.0 (30 Aug.), FableCut, WeftCut, Kaestral | young or without a mature MCP | | | | watch |
| forget | Olive (chronic alpha), Flowblade (no agent), OpenCut (rewrite, MCP promised) | | | | | |

### Sound

No single open base. Pieces: Ardour 9.8 (experimental MCP, OSC), REAPER (closed, ReaScript, 60 $), whisper.cpp and Parakeet for transcription, Fairlight in Resolve through the native MCP (volume, fades, normalisation verified). All headless except REAPER and Fairlight.

### Colour, conform, delivery

| Rank | Tool | Works | Agent | Headless | Verdict |
|---|---|---|---|---|---|
| 1 | OpenTimelineIO 0.18.1 (Apache) | yes | yes, readable `.otio` | yes | exchange base; the FCPXML adapter left the bundle in March 2026 and, tested, rewrites 1.10 as 1.8 with invented media paths: structure only, the FCPXML must be written by something maintained |
| 2 | OpenColorIO 2.5 | yes | CLI, Python | yes | colour base |
| 3 | Resolve Studio (grade, conform) | yes | yes | app open | base |
| 4 | DCP-o-matic 2.18 (CLI) | yes | yes | yes | DCP base |
| 5 | ffmpeg 9, HandBrakeCLI, whisper.cpp | yes | yes | yes | delivery base |

## 2 bis. On which machine: Mac, Windows, Linux, iPad, headless

Columns: macOS (Apple Silicon) · Windows (NVIDIA towers) · Linux · iPad · headless (renders or cuts without a screen). Source: an 86-URL platform matrix read on 13 September, current version in brackets.

| Tool | macOS | Windows | Linux | iPad | Headless | Speed measured or documented |
|---|---|---|---|---|---|---|
| Final Cut Pro (12.3) | yes, macOS 15.6+ | no | no | separate app, not at parity | no | launch 20 s; FCPXML import without a dialog |
| Motion (6.3), Compressor (5.3) | yes | no | no | no | Compressor yes (CLI), Motion no | |
| Resolve Studio (21.1) plus native MCP | yes, macOS 15+ | yes, 4 GB GPU | yes, Rocky 8.6 | yes, no scripting | partial, app required | launch 2 min; the MCP restarts its Python after 5 min idle |
| Palmier (0.9.0) | macOS 26 only, Apple Silicon | no | no | no | no, app required | under 1 s per operation, 3 XML files in 2 s, 30 s of H.264 exported in 20 s (measured) |
| Velorn (0.3.33) | yes | yes | yes | no | not documented | MCP listening 4 s after launch (measured) |
| Frontstage, Kaestral (Palmier forks) | web / no | native | web | no | partial | unknown |
| Diffusion Studio, Timeline Studio, OpenCut | web or Mac app | web | web | no | partial (CLI) | |
| Kdenlive (26.08), Shotcut (26.8) | yes | yes | yes | no | yes through melt | |
| Flowblade | no | no | yes | no | partial | |
| Blender (5.2) VSE and compositor | yes | yes | yes | no | yes, `-b` | |
| Pallaidium | not supported | yes, CUDA required | limited | no | not documented | |
| ComfyUI (0.35) | yes | yes, CUDA | yes | browser | yes, API | |
| Krita plus AI Diffusion | yes, macOS 14+ | yes | yes | no | partial (headless ComfyUI) | |
| NodeTool (0.7) | yes, macOS 12+ | yes | yes, AppImage | no | yes, CLI and Docker | |
| basketikun infinite-canvas (0.18) | browser | browser | Docker | browser | server yes | instant, rendered in the browser |
| OpenChar Studio | yes | yes | yes, NVIDIA advised | no | yes, server | |
| Natron (2.5) | Intel under Rosetta, Apple Silicon in alpha | yes | yes | no | yes, NatronRenderer | |
| HyperFrames, Remotion, Motion Canvas | yes (Node plus Chrome) | yes | yes | no | yes, CLI | HyperFrames 30 s in 16:9 rendered locally (measured) |
| A pipeline runner (OpenMontage) | yes | yes | yes | no | yes | |
| REAPER (7.80), Ardour | yes | yes | yes | no | partial | |
| OpenTimelineIO (0.18.1), OpenColorIO (2.5.2), DCP-o-matic (2.18.50) | yes | yes | yes | no | yes, CLI | |
| CapCut, Descript | yes | yes | web | CapCut beta | no | |
| After Effects (26.5), Premiere (26.5) | yes | yes | no | no | AE yes (`aerender`), Premiere no | |

What follows: the native Mac block (Final Cut, Motion, Compressor, Palmier) only runs on the laptop; anything an agent launches on a Windows tower or a headless Mac server must be in the headless list (ComfyUI, Blender, ffmpeg, melt, OTIO, Remotion, HyperFrames, the pipeline runner) or inside Resolve, the only NLE that lives on all three systems with an API.

## 2 ter. Machines and network

On the GPU towers the same night: Krita 5.3.3 plus AI Diffusion 1.53.0, the Deforum nodes for ComfyUI, and on one of them Blender 5.2 LTS with Pallaidium activated (54 plugins loaded, preferences saved). Two things an agent cannot do over ssh on Windows: restart a service it did not start, and install anything while a scheduled `winget upgrade` holds Windows Installer, which one did for twenty hours. Those steps stay manual, in front of the machine.

On the network side, with Tailscale 1.102: Serve gives each tower a stable HTTPS name for ComfyUI without opening its port to the Internet; grants replace ACLs; Tailnet Lock is worth keeping with two signers. Funnel for ComfyUI, Mullvad exits, subnet routers, Headscale (needs a 1.80 client, no parity) and a private DERP add nothing here. For 50 GB of rushes, measure a direct path against a relayed one before changing tools: rsync over ssh or SMB over the Tailscale IP, Taildrop for one-offs.

## 2 quater. Apple Motion driven by an agent

Motion 6.3 has no API, no CLI, no headless render. But its Final Cut templates (`.moti`, `.motr`, `.moef`, `.motg`) are readable XML (OZML) in `~/Movies/Motion Templates.localized/`. The robust path: validate master templates in Motion with published parameters, then let the agent clone them and change values (texts, colours, durations) to produce variants. It never synthesises a `.moti` from nothing. A community file-side prototype exists. For credits, batches and multi-format graphics, Remotion and HyperFrames in ProRes 4444 with alpha stay safer; After Effects keeps `aerender` in reserve. One unknown: whether Final Cut 12.3 refreshes dropped templates immediately is not guaranteed by Apple.

## 3. Face to face: the "agent-first" editors

| | Palmier | Frontstage | Velorn | Diffusion Studio | Timeline Studio |
|---|---|---|---|---|---|
| Model | ordered tracks, non-magnetic, layers by track order | same model (port of Palmier) | classic timeline | layers, groups, scenes, tree in code | simple tracks (visuals, overlays, captions) |
| The agent addresses | `clipId`, `trackId`, whole frames | same | MCP tools | the code itself | JSON plan, diff and apply |
| Channel | local MCP, 51 tools | agent 40 tools plus MCP 51 | 100+ MCP tools | skills plus CLI, no documented MCP | skill plus CLI plus MCP |
| To FCP/Resolve | FCPXML 1.10 to 1.14, XMEML | announced | announced, not validated | no | no |
| Open | closed since 0.8.1 | GPL-3 | GPL-3 | MPL-2 | MIT |
| Platform | macOS 26 only | web, Windows | Mac, Windows, Linux | web, Electron | PWA |
| Age, size | 2 months, 14,385 stars | 1 week, 10 stars | 475 stars, 0.3 | alpha, 2.7k stars | 808 stars |
| For me | the excerpt tool, measured | watch | test (unfinished) | read the code | ignore |

The one-hour test that settles an editor: three short clips with timecode, ask a coding agent for a 60 to 90 second cut, six splits, a track change, a title; export two FCPXML files (Final Cut target, Resolve target); success only if no cut drifts by a frame, no media has to be relinked by hand, and the clips stay editable. Palmier passed it.

## 3 bis. The canvases, tested for real (eleven tools, 35 captures)

Same protocol for each: install, launch, three images and one video placed, MCP session opened and its tools counted, a ComfyUI generation on a local tower, export, capture.

| Tool | Video plays on the canvas | MCP (tools) | Local ComfyUI | Export | Verdict |
|---|---|---|---|---|---|
| basketikun/infinite-canvas 0.18 | yes, ProRes and H.264 | 34 | no (OpenAI or Gemini endpoints only) | zip of JSON plus files, no timeline | best canvas and best canvas MCP; zero editing, zero native ComfyUI |
| BeatDesign 0.2.3 | yes H.264, ProRes refused above 100 MB | 29, with expected revision and receipts | no (its own API only) | server-rendered MP4, no FCPXML | best agent-driven timeline; single provider |
| NodeTool 0.7 | no (images per shot) | 11 | possible by node, not run | zip of stills plus storyboard | storyboard to stills to clips; no free canvas; takes the foreground on launch |
| Jaaz 1.0.30 | no | 0 (HTTP API) | yes natively, but driven by a chat LLM that is absent | Excalidraw JSON | asleep since March 2026 |
| Excalidraw plus MCP | no (no video) | 26 | n/a | .excalidraw | whiteboard for an agent; `describe_scene` worth copying |
| AFFiNE | no | 105 | n/a | Markdown, PDF | too heavy, no video |
| OpenChar 1.3.21 (ex Inline) | not verified | 0 (REST) | no (built-in torch engine) | zip, takes through `/v1/takes` | the take as an object, worth revisiting |
| Raccord 1.4.0 | not verifiable | 145 | no (kie.ai only) | refused without a generated clip | richest direction grammar; nothing without a key |
| DX-OS | closed client | announced | announced | unknown | proprietary successor of hero8152, out of scope |
| **A local shot-store board** | placed yes, **plays no in Chromium** (10-bit HEVC proxy) | 7 | **yes, the only local generation that succeeded** (Flux img2img on a tower, 15 s, take placed and linked) after writing the workflow: the four shipped ones were placeholders | FCP7 XML plus board JSON | the only complete local chain, two blocking holes |
| Martini | yes (documentation) | 36 hosted | no | XML plus zip | the reference for the 3D set, nothing installable |

**Verdict on the home-made board: improve, do not abandon.** No tested tool combines a named take, ComfyUI as backend, an export to an editor, heavy media and local execution. In order: proxies in 8-bit H.264 (or the video is black in Chromium), one real ComfyUI workflow per engine (Flux written that night, Wan, LTX and VACE remain), the MCP inside the repository with the borrowed verbs, OTIO and FCPXML 1.x. If the first two are not done quickly, the comparison tips towards basketikun (canvas) plus an adapter to ComfyUI plus BeatDesign (timeline), at the price of two foreign projects and no notion of a take.

The gestures worth borrowing, seen for real: board state and batch operations with selection (basketikun); expected revision, command id and a receipt on every write, wall diagnostics, next take from the last frame (BeatDesign); a text description of the scene, checkpoints and a board diff (Excalidraw, Raccord); a preview of the ComfyUI payload before sending, and a lint (Raccord); a shot vocabulary and "assemble" only when every scene has a take (NodeTool, Raccord); a ComfyUI proxy and a store of JSON workflows as engines (Jaaz); an overview in one call, two-step upload, a stack of variants (Martini).

**Krea.** I read one of my own Krea Agent sessions end to end and I am leaving the platform: price, and moderation on images that are not a problem. What it does well is reproducible at home: one reasoning model as the brain, the loop "generate, look, correct", several models in one gesture, a measurement (a grain script) when a measurement is needed. The target stack: a coding agent as the brain, the local board as the surface, ComfyUI on the towers first, a paid API only for models with no local equivalent and only on an explicit go, every open model (Ollama, OpenRouter) pluggable into the board.

## 4. Dead or alive

Reading rule, corrected after a detailed report with dated sources: a repository without commits is not dead if it still runs and nobody has reproduced its gesture. "Dead" only applies to what is archived, broken without a fork, or replaced by the same gesture elsewhere.

| Tool | State on 14 September 2026 | Replaced by |
|---|---|---|
| Storyboarder (Wonder Unit) | dead, last commits June 2022 | Blender VSE plus Grease Pencil, Resolve |
| hero8152 Infinite-Canvas | stopped 28 August 2026 | basketikun |
| artokun/comfyui-mcp | archived 10 September 2026 | the official ComfyUI MCP |
| Comfy-Org/comfy-cloud-mcp, Comfy-Org/desktop | archived (June, May 2026) | official MCP, Comfy-Desktop |
| Olive | chronic alpha | nothing to do |
| Jaaz | non-OSI licence, repository quiet since March 2026 | basketikun, a local board |
| OpenCut | in rewrite, MCP not shipped | wait |
| Palmier source code | GPL up to 0.7.6, closed after | Frontstage or Kaestral if the forks hold |
| SpliceKit | alive on Final Cut 12.3 once the two open pull requests are applied and the patch is run by hand; the maintainer is quiet since May | nothing yet; FCPXML by file remains the base route |
| otio-fcpx-xml-adapter | last commit June 2024, out of the bundle, rewrites 1.10 as 1.8 with invented paths | any maintained FCPXML writer |
| Kdenlive, Shotcut, OpenShot | alive, 2026 versions | nothing, but no living channel except shotcut-mcp |
| deforum/sd-webui-deforum | **stable and frozen** on A1111 1.9 (May 2024), artistically alive: no 2026 video model reproduces the recursive img2img loop with 3D reprojection and per-frame schedules | keep a frozen A1111 1.9 environment on one tower; qualify the official `deforum/deforum-comfy-nodes` port (May 2026); Parseq as sequencer; the Forge fork stays experimental. Wan Camera, Uni3C and VACE drive a camera, not this loop |
| ProPainter | stable without commits, old stack | stays the reference for video removal; compare ComfyUI_ProPainter_Nodes and DiffuEraser on a 5090 before changing |
| FizzNodes | stable, partly broken on the 2026 stack | **no replacement**: KJNodes does not do `BatchPromptSchedule`; keep for old graphs |
| AdvancedLivePortrait | broken on recent ComfyUI | the `sheepbooy` fork for manual expression sculpting; official LivePortrait, OmniHuman, X-Portrait only for video or audio driving |
| FFCreator | stable without commits | keep for existing scripts; HyperFrames or Remotion for new work |
| Steerable-Motion (banodoco) | slowly alive, Wan/VACE path added | archive the AnimateDiff graphs; LTX multi-guides, Wan first/last, FramePack for new work |
| OTIO-AVFoundation, Text_to_Video_Markers, reuelk/pipeline | finished, not abandoned | keep for their case; fcp-mcp-server (DareDev256) or fcp-mcp (dreliq9) to write FCPXML 1.13 |

## 4 bis. The morning after: Pallaidium in remote mode, HyperFrames Studio, and where I land

Two more tests before dawn changed two rows.

**Pallaidium works in remote mode, on a Mac.** The add-on ships three "remote backend" connectors in plain Python, no pip: a ComfyUI adapter, a fal.ai adapter and a mock. The adapter speaks a small `/v1` contract (health, models, image, video, speech, jobs, files) and forwards each model to a ComfyUI workflow file dropped in a folder, parameters injected by node title. Started on the laptop against the ComfyUI of an RTX 5090 tower, Blender 5.2.1 headless registered five remote models, and `sequencer.generate_image` placed a FLUX schnell frame in the VSE in 6.9 seconds, rendered from the timeline afterwards. Two defects on the way, both patched locally and worth an upstream report: the operator still imports `torch` before it looks at the backend, and the remote plugin returns a file path where the operator expects a PIL image. The row moves from "to test on a tower" to a base for phase 5 whenever the work is in Blender; the tower computes, Blender only carries the adapter. The same `/v1` contract is reusable by any script, and the fal adapter covers the "paid API only when nothing local exists" case.

![The image generated through Pallaidium's ComfyUI adapter on a remote tower, from a headless Blender on the laptop](assets/2026-09-14/pallaidium-remote-flux-torrent.jpg)

![The same frame rendered from Blender's VSE timeline, where Pallaidium placed it as an image strip](assets/2026-09-14/blender-vse-remote-strip.jpg)

**HyperFrames Studio exists and runs locally.** `npx hyperframes preview` opens a visual editor in the browser, no HeyGen account: storyboard, live preview, a clip timeline, layers and a design inspector, a "describe a change to the agent" box, gesture recording and export. It does not change the verdict (a page is still a page), it lowers the cost of the last ten percent, the timing pass a human wants to do by eye.

![HyperFrames Studio on the repository presentation rendered the day before](assets/2026-09-14/hyperframes-studio.png)

**Where I land, phase by phase**, now in the last column of the first table. Three calls worth spelling out. Writing: Obsidian alone, Markdown on disk the agent reads and writes natively, with Notion as a one-way mirror for the team, never the place where the text moves. Animatic: Resolve when the boards are already images, because the animatic then becomes the timeline of the cut; Blender when the line or the camera matters, because a blocked-out set gives every angle for free and its grey render is the best input a video model can get. Compositing: Fusion by default, since the shot stays in the timeline where it is judged; Blender's compositor when the passes come from Blender; Natron the day a stable build lands on Apple Silicon, because an open, headless compositor on the towers is what the social line needs.

**Palmier and your own keys.** Palmier Pro generates only through its credits (250 at sign-up, then a subscription), with no personal key and no ComfyUI. The fork that does it is Frontstage (GPL port of Palmier 0.7.6): fal.ai key for generation, OpenRouter key for the agent, Whisper local, same MCP port; no ComfyUI either, an unsigned Windows binary, web or build-from-source on a Mac. The route without credits or a fork: generate outside (the board, or Pallaidium's `/v1` contract to ComfyUI or fal) and place the media in Palmier through its MCP (`import_media`, `add_clips`). Same gesture, no credits.

## 5. The one-hour tests, in order

1. **Palmier**: done, passed. The excerpt line has its tool.
2. **Velorn**: same protocol, FCPXML out to Resolve. Started, not finished.
3. **Pallaidium**: done, passed in remote mode from the Mac (see 4 bis). Video through `ltx-2.3-i2v` still to run: the bundled workflow expects an fp8 checkpoint and two LoRAs the tower does not have.
3 bis. **Wan2GP and SwarmUI**: simple fronts over ComfyUI, one hour each on a tower.
4. **basketikun infinite-canvas** in Docker: one media placed by a coding agent through its MCP, reopened. Done.
5. **OTIO round trip** between the local board and Resolve. Planned.

What this ranking does not change: Final Cut for the hand cut, Resolve Studio for the agent and the conform, a home-made board and pipeline runner as the local layer, HyperFrames and Remotion for motion and demos. A working position, held as long as it works.
