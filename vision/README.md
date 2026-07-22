# Vision

Sample notebooks for Sarvam AI's vision (document understanding / OCR) model packages on the AWS Marketplace.

All vision listings share one simple interface — the request body is the **raw document bytes** and the response is a **zip archive** with the recognized content — exposed through three inference modes on Amazon SageMaker:

1. **Real-time inference** — POST the raw PDF to a SageMaker endpoint (`Content-Type: application/pdf`) and receive the result archive synchronously. Best for small documents (payload ≤ 6 MB, response within 60 seconds).
2. **Asynchronous inference** — upload the PDF to S3 and invoke an async endpoint; the result archive is written back to S3 (payloads up to 1 GB, processing up to 1 hour).
3. **Batch transform** — OCR a folder of PDFs from S3 in one offline job (each S3 object is one raw PDF).

The exact output schema can differ between models — refer to each model's folder, which is self-contained (notebook + sample inputs + expected outputs).

## Models

| Model | Description | Notebook |
|---|---|---|
| [OCR 3B](ocr3b/) | Document OCR: PDF in, Markdown + per-page layout metadata out. Runs on a single NVIDIA L40S GPU (`ml.g6e` family). | [ocr3b-vision-Model.ipynb](ocr3b/ocr3b-vision-Model.ipynb) |
