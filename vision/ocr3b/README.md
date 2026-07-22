# OCR 3B — Vision

Sample notebook and data for the **OCR 3B** model package on the AWS Marketplace: Sarvam AI's document OCR model — PDF in, structured text and layout out.

- **Notebook**: [ocr3b-vision-Model.ipynb](ocr3b-vision-Model.ipynb)
- **Real-time / async instance type**: `ml.g6e.xlarge` (1× NVIDIA L40S)
- **Batch transform instance type**: `ml.g6.xlarge`

## Interface summary

The same request/response schema applies to all three inference modes:

- **Request**: the raw bytes of the PDF, `Content-Type: application/pdf`. No other parameters.
- **Response**: a zip archive, `Content-Type: application/zip`.

Equivalent to:

```bash
curl -X POST https://<endpoint>/invocations \
     -H "Content-Type: application/pdf" -H "Accept: application/zip" \
     --data-binary @document.pdf -o result.zip
```

### Response archive

| Archive member | Description |
|---|---|
| `document.md` | The full recognized document as Markdown, pages separated by `---`. Tables are rendered as Markdown tables. |
| `metadata/page_NNN.json` | One file per page: page dimensions (`image_width`, `image_height`, in rendered pixels) and the detected layout blocks. |

Each entry in a page's `blocks` list:

| Key | Description |
|---|---|
| `block_id` | Unique identifier of the block. |
| `coordinates` | Bounding box `{x1, y1, x2, y2}` in rendered-page pixels. |
| `layout_tag` | Block type, e.g. `headline`, `paragraph`, `ordered-list`, `header`, `footer`. |
| `confidence` | Confidence of the layout detection. |
| `reading_order` | 1-based position of the block in reading order. |
| `text` | Recognized text of the block. |

### Choosing an inference mode

| Mode | API | Limits | Use for |
|---|---|---|---|
| Real-time | `invoke_endpoint` | payload ≤ 6 MB, response ≤ 60 s | small documents, interactive use |
| Asynchronous | `invoke_endpoint_async` (S3 in/out) | payload ≤ 1 GB, processing ≤ 1 h | large / many-page documents |
| Batch transform | transform job | `MaxPayloadInMB` per object | offline folders of PDFs |

For batch transform, each S3 input object is one raw PDF (sent to the container as-is with the job's `ContentType: application/pdf`); each produces one `<object name>.out` file containing the result zip.

## Data files

```
data/
├── input/sample_document.pdf        2-page sample PDF (headings, paragraphs, list, table)
└── output/
    ├── sample_output.zip            expected response archive for the sample PDF
    └── extracted/                   the same archive, extracted for browsing
        ├── document.md
        └── metadata/page_001.json, page_002.json
```

The expected output was captured from a live SageMaker endpoint running this model package.
