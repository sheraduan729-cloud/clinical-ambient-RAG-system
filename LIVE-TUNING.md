# Live transcription tuning — 2026-11-10

Observed issue: the live Transcript view contains a negation split across segments (“I'm not / I'll take…”), while the user reports the post-recording Speakers & playback result is correct. No original audio was reviewed in this change; a screenshot does not establish the acoustic root cause.

The two views use separate inference paths: live `gpt-live-transcribe`, versus a full-recording `gpt-4o-transcribe-diarize` request. Neither output is automatically authoritative; compare with retained audio.

Changes applied to the intelligence backend:

- Default live transcription delay changed from low to medium.
- `OPENAI_REALTIME_TRANSCRIBE_DELAY` supports minimal/low/medium/high/xhigh for controlled comparisons; invalid values are rejected.
- Added language-appropriate clinical context and limited domain keyword hints. No expected patient name, temperature, duration or full answer is embedded in the prompt.
- Preserve provider output; no post-hoc replacement of negation, quantities, or patient facts. Existing redaction/encryption still applies before display.

No frontend changes were made, preserving the currently open recording/results. The existing 700 ms client-side pause threshold and 30-second hard turn boundary are unchanged. Either could contribute to a boundary issue; assess with recorded test audio before changing these independently.

71 backend tests passed, including six new cases for context/configuration, delay comparisons, invalid settings, and fake-cloud end-to-end preservation of a negative medication statement and a non-template dosage. Three local service-chain tests passed after restart. These do not establish acoustic accuracy or measure quality improvement. No paid API or microphone test was run.

The intelligence service was restarted only after checking that it had no established TCP connections. The current page's results remain in browser memory. Backend in-memory history indexes and inference caches were cleared by the restart: refresh the history list and re-import records if testing RAG afterward; do not refresh the whole page if retaining the current audio for comparison.

Next validation: compare the same fictional recorded audio with low versus medium/context configurations. Include a pause after a negation, a statement near the 30-second boundary, medication names and quantities. Review audio and manually score missed/changed negations, dropped words and latency. A new live session uses the updated configuration; an already displayed transcript will not change.

Official guide: https://developers.openai.com/api/docs/guides/realtime-transcription
