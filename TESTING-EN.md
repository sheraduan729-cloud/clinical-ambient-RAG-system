# English feature test

Use the local app at http://127.0.0.1:5174/. Refresh once. The interface and default recognition language should be English. Use two different people for speaker analysis. Use fictional data only.

## 1. Free local checks

- Privacy & data → Test redaction & encryption (local only).
- Expected: Alex Morgan, the phone number and email are replaced with English placeholders. “37.8 degrees” and “No chest pain” remain. The original is reported encrypted. This does not prove complete PHI coverage.
- Back to consultation → Import 3 sample records (visible directly in Relevant history).
- Custom search → Test tools → Use sample query → Search patient history.
- Expected: Drug allergy history and Respiratory follow-up, with source references; no Knee follow-up.
- Try “toothache”: no matching records should be returned for these samples.
- Switch patient to DEMO-B: it must not return DEMO-A records. Switch back and refresh the record list if needed.

## 2. Record this fictional dialogue (cloud API charges apply)

Check both consent options: sending test audio to OpenAI, and keeping audio for playback/speaker analysis. Confirm Language = English. Start recording, alternate speakers naturally, and leave a short pause between turns. Keep the entire recording under 120 seconds.

**Doctor:** Hello. What brings you in today?

**Patient:** My name is Alex Morgan. I have had a cough for three days. It is worse at night.

**Doctor:** Have you had a fever, chest pain, or trouble breathing?

**Patient:** My temperature was 37.8 degrees this morning. I do not have chest pain or trouble breathing.

**Doctor:** Do you have a history of asthma? Are you taking any medication?

**Patient:** I have a history of asthma. I am allergic to penicillin. I am not taking any other medication.

Stop & confirm. Wait for final text. Expected: English text, the contextual name masked, “37.8”, negation and “penicillin” checked manually. Temporary partial words are deliberately hidden.

## 3. Speaker roles and history

- Speakers & playback → Identify speakers (extra API charge). This sends the retained full recording separately.
- Expected: two voices when distinguishable. Role suggestions may be Doctor and Patient, but must remain suggestions until confirmed manually. A single person reading both roles is not a valid two-voice test.
- Listen back before confirming roles or accepting the text. ASR and diarization may differ; neither is guaranteed accurate.
- Relevant history defaults to the confirmed conversation. If a sample query or manual text is selected, click Use current conversation first. Find history for this conversation should retrieve the asthma/allergy records.
- New session clears conversation, role selections, query and results, while retaining this patient's imported records in the current workspace. Switching patient clears the active conversation and changes the retrieval scope.

## Optional reconnect test

While recording, open Connection details & reconnect test → Simulate disconnect. Continue briefly, then Stop & confirm. The queue should drain and the session should finish or report a clear failure, without silently starting a new session. A backend restart is not a recoverable browser disconnection.

## Implemented validation

English UI/default language, English sample records/query, local English redaction and retrieval are verified in-browser. 20 frontend tests pass, including English dialogue role inference; both pages build successfully. No paid recording or real microphone test was run automatically in this change. Patient codes are a prototype retrieval scope, not authentication. Rule redaction can miss identifiers or over-redact clinical numbers.
