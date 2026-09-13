**English** · [Français](2026-09-13-editing-with-agents-by-type-of-work.fr.md)

# Editing with agents, by type of work

*Field note, 13 September 2026. Everything below was checked on my own machines that day. Where I only read a page, I say so.*

I cover almost every editing situation there is. A feature-length documentary cut by hand over months. A hybrid feature in development where part of the image is generated. Social clips and teasers in three aspect ratios. Demos of my own command-line setup. Titles and motion. Exhibition loops. And the tools I build for all of that. For a while I looked for one stack that would serve all of them. There is none, and the search itself was the mistake. The question that actually helps is: **for this type of work, where is the cut judged, what does the agent read and write, and through which channel?**

## Tracks, layers, and the question underneath

A thread I read that week made the case that AI agents struggle with the track model of a classic editor (Premiere, the Edit page of Resolve) and do better with a tree of objects, the way After Effects nests layers or a browser exposes the DOM. Diffusion Studio moved from tracks to layers for exactly this reason. The point is right for a language model writing code. It is also older than it looks: Final Cut dropped numbered tracks in 2011 for a spine with connected clips, and Fusion has been a node graph forever.

The tracks-versus-layers framing hides the real question, which is **which object the agent reads and writes, and through which channel.** There are three channels, and I use all three:

1. **A live API on the open application.** DaVinci Resolve, through its scripting API and, since 21.1, an MCP server shipped by Blackmagic. The agent addresses a timeline clip as an object. At this level the track debate disappears.
2. **An exchange file the human imports.** FCPXML for Final Cut, OpenTimelineIO between everything. Asynchronous, structure only, never colour or effects. Final Cut has no scripting API at all (`sdef` on the app returns nothing), so this is its only door.
3. **Code that is the document.** HyperFrames (HTML and CSS), Remotion (React), Motion Canvas (TypeScript), Diffusion Studio's engine (TypeScript). The agent is at home here, and nobody cuts a feature in HTML.

An author film lives in channels 1 and 2. Communication lives in channels 1 and 3. The gap everyone names, one tool that cuts rushes, composites like After Effects, delivers a DCP and is owned by the agent end to end, is still open, and I do not need to close it before my next first assembly.

## One stack per type of work

| Type of work | Where the cut is judged | Where the agent acts, and through which channel | What the agent does | What it never does |
|---|---|---|---|---|
| **A. Author feature: documentary and narrative editing** | Final Cut Pro. My cut, my eye, and an editor who lives in Final Cut, collaborating on a shared library | File channel: FCPXML written by my own indexer, OpenTimelineIO; local transcription; notes and journal | Transcribes, indexes, proposes selects and a first assembly with markers and roles, keeps the documentation current | The master. The meaning. The final cut |
| **B. Hybrid: animation, compositing, VFX, generated image and sound** | DaVinci Resolve Studio, 32-bit float ACES timeline, Fusion for nodes. Runs on Mac and Windows, so also on the NVIDIA towers. A VFX team distinct from the Final Cut team. Less narrative scene cutting, more per-shot detail | Live API (native MCP, scripting); a shot store that versions every take; a verb layer that drives the NLE; ComfyUI on the towers | Generates on the towers, places a version at timecode, disables the previous one, writes the sidecar and the C2PA manifest | Choosing the take that stays |
| **C. Social: excerpts, teasers, multi-format** | The agent's output, read by me; a pass through Final Cut only when the film's grain matters | Headless: pipeline runner, ffmpeg, local transcription; channel 3 for chrome | Cuts, subtitles, reframes 16:9 / 9:16 / 1:1, produces variants, exports | Publishing without my go |
| **D. Tech demos of my own setup** | Direct render, no NLE | Screen capture plus Remotion; HyperFrames for repository and page videos | Writes the script, generates the chrome, the fake terminal, the duration variants | Nothing excluded: this is where the agent can do everything |
| **E. Motion, titles, chrome** | Motion (Apple) when it enters a film; HyperFrames or Remotion for web and social | Channel 3 | Produces standalone sequences I reimport | Deciding the film's graphic design |
| **F. Installation, exhibition loop** | Resolve for export, loop and calibration | Live API for exports and conforms | Encodes, derives, checks delivery formats | The installation itself |
| **G. Tool research and development** | No editing: code | Diffusion Studio's engine as a codebase to read, small local editors as agent labs | Prototypes | Becoming yet another source of truth |

Four rules fall out of the table:

- **One rush crosses several rows.** An interview is in A (the film), then in C (the excerpt), then in E (the excerpt's chrome). Three different edits with three levels of delegation. One pipeline would serve none of them well.
- **The shared grain between A and C comes from presets.** The same LUTs, the same Motion titles, the same Final Cut roles, stored once and reused by the social line.
- **The shorter the edit, the more the agent does.** In D it does everything, in A it prepares. That is the only rule that governs the choice of tool.
- **Between A and B, the split is by sequence, never by shot.** FCPXML and OpenTimelineIO carry structure and nothing else. A shot that travels back and forth between the Final Cut team and the Resolve team gets conformed twice. A hybrid sequence leaves whole for B and comes back to A as one rendered media file.

## What I measured that day

**Resolve Studio 21.1.0.14, native MCP, five scripted tests.** Connection and project listing: yes. Sandbox: `import os` is refused, file access does not exist. A sequence of 25 float32 EXR frames imported as one 32-bit clip and placed at timecode on a timeline created by script, next to a ProRes and a single EXR: yes. `ValidateDCTL` accepts a valid kernel and rejects garbage with a readable message. `AutoAlignClips` and `NormalizeAudioLevel` exist on the timeline; `AddTransition`, `SetFades`, `SetSpeed`, `AddVersion`, `AddTake`, `AddFusionComp`, `SetCDL` and `SetClipEnabled` exist on the item. Disabling then re-enabling a clip by script works, which is the exact verb row B needs.

One trap worth knowing: a modal dialog in Resolve (in my case a cache location pointing at a disk that no longer exists) makes every import return `0` and the current page return `None`, with no error. The MCP does not see the dialog. Before concluding that an import fails, list the application's windows.

**HyperFrames 0.8.37** (HeyGen, Apache 2.0). Renders an HTML page with GSAP timing to MP4 through headless Chrome and FFmpeg. On a real case, a 30-second presentation of one of my public repositories in 16:9 and 9:16, written by the agent, rendered locally without any HeyGen key. Two renders of the same file are byte-identical. The package is heavy (371 MB, onnxruntime, sharp, puppeteer-core), the system Chrome must be pointed at explicitly, and it fails inside a sandboxed terminal because Chrome refuses the macOS sandbox. It enters the stack for rows D and E, and stays out of editing.

**Remotion.** A 20-second demo, dark fake terminal, one title, rendered in 1920x1080 and 1080x1920 locally, deterministic, clean. The only friction is Chromium, to preinstall and point at for a guaranteed offline render. Free for a sole trader. Yes for row D, and an agent drives it better than HyperFrames: components compose, a monolithic page gets dense fast.

**A pipeline runner's `screen-demo` recipe** (OpenMontage). The editorial preparation, checkpoints and artefacts are usable; the composer does not render and its fallback output is not deliverable. To repair before counting on it for row C.

## The startups, checked on the web the same day, not tested

- **Palmier** (palmier.io): a native Mac NLE, macOS 26, with a local MCP server for coding agents, local transcription, FCPXML 1.10 to 1.14 and XMEML export. Version 0.9.0 on 9 September 2026, around 14,400 stars. GPLv3 up to 0.7.6, proprietary binaries since 0.8.1 on 28 August. The only new name worth a test for row C, agent-driven cutting with a Final Cut round trip. Never the master.
- **Descript** has an official MCP connector since 11 June 2026 (API in open beta since 16 April). Useful for interviews and text-based derivatives, row C.
- **Mosaic** (YC W25): REST API, webhooks, a published skill for coding agents, Premiere project output. Cloud, agency-scale. A local pipeline runner already does what I would copy.
- **Cardboard** (YC W26), **Narrative** (YC F25), **Kestroll** (YC S25): real, active, SaaS. No public API, MCP or skill at Cardboard and Narrative; Kestroll exports FCP7 XML and FCPXML. Laboratories.
- **Timeline Studio**: MIT, 1.0.4 on 17 August 2026, skill and CLI, MCP only planned, no NLE exchange documented. A laboratory, below Palmier.
- **ChatCut**: an MCP plugin for coding agents, Premiere and Resolve export, no Final Cut. Laboratory.
- **Diffusion Studio**: the `core` engine is TypeScript under MPL-2.0 with WebCodecs rendering; the editor is in alpha since May 2026 with a CLI, no MCP and no NLE exchange. An engine to read, not a timeline.

## Before the timeline

A second thread the same day was about infinite canvases (Firefly Boards, Flora, Jaaz, Inline Studio) and animation pipelines. Three things hold. The canvas comes *before* the timeline. A film is time, and a 2D wall carries neither rhythm nor continuity; the canvas is where you compare worlds (a real location next to a generated sky, three lighting setups, a wide shot against an insert) and lower the cost of being wrong before a camera is switched on or a generation is paid for. Wall and factory are different tools: a board is for placing and comparing, a graph is for wiring and re-running, and the graph only earns its place once a recipe repeats. And for animation with generated shots, the animatic is the gate: drawing (the line is the LoRA's dataset, not decoration), LoRA, bible, boards, animatic with temp sound, key frames, motion, edit. Generating video before the animatic and the LoRAs are locked is paying to discover the film does not exist yet.

## What I keep

A working position, held as long as it works: no single house. Final Cut is where I cut by hand and where a Final Cut editor can join. Resolve Studio is the host for anything the agent must do alone on a timeline, and for conform, colour and hybrid work, on Mac and on the Windows towers. FCPXML and OpenTimelineIO are the bridge, and they carry structure only. HyperFrames and Remotion make the motion and the demos. A local pipeline runner replays my recipes for social. The day a tool cuts, composites and delivers better than that bridge, the position changes.
