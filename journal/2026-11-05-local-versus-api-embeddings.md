[English](2026-11-05-local-versus-api-embeddings.md) · [Français](2026-11-05-embeddings-locaux-contre-api.fr.md)

# 5 November 2026: local versus API embeddings

Logbook. A bench run with Claude Code to find out whether a fully local search over working archives holds up against Google's embedding model through its API: the case, the protocol, the results question by question, the cost, what is still open.

## The case

Working archives (reports, notes, recordings, images) where a specific passage has to be found again. Part of these archives must not leave the machine. The question: how far does a search that runs entirely on the machine go, and what is gained by sending the corpus to an API.

## The two models

mediarag is a local tool: it splits files into chunks, computes the vectors, stores them in a SQLite database and answers searches over MCP. It ships with EmbeddingGemma 2, Google's open model (Apache 2.0) released on 6 October 2026: 740 million parameters, 768 dimensions, 8,192 tokens, text, image and audio. It runs on the CPU, offline once downloaded. Published score on MTEB multilingual v2: 61.36.

Gemini Embedding 2 (`gemini-embedding-2`) is Google's multimodal embedding model through the API, generally available since 22 April 2026. It takes text, image, video, audio and PDF, up to 8,192 tokens, with vectors from 128 to 3,072 dimensions. Published score on MTEB multilingual: 69.9. Neither has a published score for French on its own.

The comparison therefore sets two models side by side, one inside a complete tool, the other behind an API that needs a script around it. mediarag cannot call Gemini: its code has no model option.

## The protocol

The corpus: a folder of working reports from a single night, judged non-sensitive, 9.9 MB. 71 files: 64 texts (Markdown, HTML, plain text), 6 recordings of a voice agent (120 seconds in total), 1 image. mediarag cut them into 1,531 chunks: 1,509 text, 20 audio excerpts, 2 image entries.

On the Gemini side, the same 1,531 chunks, read from mediarag's database, audio excerpts cut at the same timestamps. 768-dimension vectors, as with EmbeddingGemma 2. Task instructions as recommended by Google (`title: … | text: …` for documents, `task: search result | query: …` for questions). Similarity search only.

One difference of method remains: mediarag combines similarity and keywords, while Gemini only had similarity here.

Five questions, each with an answer checked in the files. For each, the top three results are read.

## Question by question

1. The measured latency of a local voice agent. The answer (0.90 seconds median, from the end of the sentence to the first sound) is in a measurement report. Both miss. mediarag returns the brief that asked for the latency to be measured. Gemini returns a research note quoting latencies found elsewhere (400 to 700 ms). The report that gives the measurement is in neither top three.

2. The fastest tool in a local model bench. Both succeed, each on a different passage of the same report: the conclusion for mediarag, the timing table for Gemini.

3. Finding one specific exchange among the voice agent recordings (audio search). Gemini succeeds: the right exchange comes first. mediarag puts another exchange first and the right one second and third. In both cases the result is an audio excerpt with its timestamps and no words: to know what is said, it has to be listened to or transcribed.

4. The size of a system cache in a disk cleanup report. mediarag succeeds: the passage carrying the figure comes first. Gemini first returns a passage of the same report without the figure, then the right one second.

5. A photo of a film editor's desk (image search). Both succeed.

Same score on both sides: 3 hits, 1 half, 1 miss, on different questions.

## What each approach misses

Question 1 is missed on both sides for the same reason. A document that talks about latency in terms close to the question ranks above the report that gives the number. Here, the brief resembles the question more closely than the report with the figures does.

mediarag ranks audio less well. It is better at finding the passage that holds the exact figure, probably thanks to its keyword component.

Gemini ranks audio better and misses the passage with the figure when a neighbouring passage of the same document is closer to the meaning of the question.

The 8.5-point gap on MTEB multilingual does not show on this bench. Five questions are not enough to separate two models.

## Time and cost

| | mediarag (EmbeddingGemma 2) | Gemini Embedding 2 |
|---|---|---|
| Indexing the 1,531 chunks | 496 s on a laptop CPU | 33 s, in batches of 50 requests |
| Search | 0.97 s for the first one (model loading), 0.03 s after | |
| Cost | $0 | about $0.12; under $0.0001 for the five questions |
| What leaves the machine | nothing | the text of the 64 files, the two minutes of audio and the image |
| Disk footprint | about 1.8 GB (Python environment, model, 11 MB index) | |

The cost was announced before launch, with a $0.50 cap and an automatic cut-off set at $0.45, never reached. The prior estimate, at the price Google published on 30 October 2026, came to about $0.12 ($0.20 per million text tokens, $0.00016 per second of audio). During the run, the counter overcounted (one token per three characters) and showed $0.135. The first batch was billed 17,555 tokens against 19,581 estimated, 10% less.

On the paid tier, Google's pricing page states that data is not used to train models. On the free tier, Google reserves the right to use it to improve its products.

Gemini indexes about 15 times faster than the local model, for a few cents.

## Open

- Index the transcripts of the recordings as text, next to the audio, so that search also returns the words.
- Add a reranker, or a keyword component on the Gemini side, for questions that expect a number, then rerun question 1.
- Rerun the bench with more questions to measure the real gap between the two models, in French in particular.
- Plugging Gemini Embedding 2 into mediarag would require changing the tool.
