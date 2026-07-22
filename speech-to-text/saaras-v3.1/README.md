# Saaras v3.1 — Speech-to-Text

Sample notebook and data for the **Saaras v3.1** model package on the AWS Marketplace: Sarvam AI's speech-to-text model for Indian languages and English.

- **Notebook**: [saaras-v3.1-speech-to-text-Model.ipynb](saaras-v3.1-speech-to-text-Model.ipynb)
- **Real-time instance type**: `ml.g6e.xlarge` (1× NVIDIA L40S)
- **Batch transform instance type**: `ml.g6.xlarge`
- **Model identifier in API payloads**: `saaras:v3` (the container rejects `saaras:v3.1` — v3.1 is the packaged revision, not a separate API identifier)

## Interface summary

### Real-time inference (`InvokeEndpoint`)

Request: `multipart/form-data` body.

| Form field | Required | Description |
|---|---|---|
| `file` | yes | Audio file to transcribe (e.g. WAV). |
| `model` | yes | `saaras:v3` |
| `with_timestamps` | no | `"true"` to include segment timestamps. |

Response: `application/json` with `request_id`, `transcript`, `language_code`, `language_probability`, and (when requested) `timestamps` — see [data/output/real-time/sample_response.json](data/output/real-time/sample_response.json).

### Real-time streaming (`InvokeEndpointWithBidirectionalStream`)

JSON text frames over SageMaker's HTTP/2 bidirectional streaming API (client: [`aws-sdk-sagemaker-runtime-http2`](https://pypi.org/project/aws-sdk-sagemaker-runtime-http2/)).

- Client → server: `{"type":"config"}`, then `{"audio":{"data":"<b64 PCM>","sample_rate":16000,"encoding":"audio/wav"}}` per ~100 ms chunk, then `{"type":"flush"}`.
- Server → client: `{"type":"events",...}` (VAD signals) and `{"type":"data",...}` (transcripts) — see [data/output/streaming/sample_messages.jsonl](data/output/streaming/sample_messages.jsonl).
- Session parameters (`model`, `language-code`, `sample_rate`, `vad_signals`) go in the URL-encoded query string (`ModelQueryString`).
- Audio must be 8 kHz or 16 kHz mono 16-bit PCM. Send frames with `data_type="UTF8"`, and keep the input stream open after `flush` until the final transcript arrives.

### Batch transform

Each S3 input object is one fully serialized `multipart/form-data` body with a fixed boundary; the job's `ContentType` is `multipart/form-data; boundary=<boundary>`. Output is one JSON file per input (same schema as real-time) — see [data/output/batch/payload.multipart.out](data/output/batch/payload.multipart.out).

## Data files

```
data/
├── input/
│   ├── real-time/sample_audio.wav        18.7 s English WAV (48 kHz mono)
│   ├── streaming/sample_audio_16k.wav    same clip, 16 kHz mono 16-bit PCM
│   └── batch/payload.multipart           pre-serialized multipart body (fixed boundary)
└── output/
    ├── real-time/sample_response.json    expected real-time response
    ├── streaming/sample_messages.jsonl   expected streaming messages
    └── batch/payload.multipart.out       expected batch transform output
```
