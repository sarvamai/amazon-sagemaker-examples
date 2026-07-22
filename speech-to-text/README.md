# Speech-to-Text

Sample notebooks for Sarvam AI's speech-to-text model packages on the AWS Marketplace.

All speech-to-text listings expose up to three inference modes on Amazon SageMaker:

1. **Real-time inference** — `multipart/form-data` POST with an audio file to a SageMaker endpoint; the response is a JSON transcript (optionally with timestamps).
2. **Real-time streaming inference** — bidirectional streaming over SageMaker's HTTP/2 `InvokeEndpointWithBidirectionalStream` API: send base64 PCM chunks as JSON text frames, receive voice-activity events and transcripts on the same connection.
3. **Batch transform** — offline transcription of pre-serialized multipart payloads from S3.

The exact request/response schema can differ between models — refer to each model's folder, which is self-contained (notebook + sample inputs + expected outputs).

## Models

| Model | Description | Notebook |
|---|---|---|
| [Saaras v3.1](saaras-v3.1/) | Speech-to-text for Indian languages and English, with timestamps and streaming support. Runs on a single NVIDIA L40S GPU (`ml.g6e` family). | [saaras-v3.1-speech-to-text-Model.ipynb](saaras-v3.1/saaras-v3.1-speech-to-text-Model.ipynb) |
