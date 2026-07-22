# Sarvam AI — Amazon SageMaker Examples

Sample notebooks and reference data for using [Sarvam AI](https://www.sarvam.ai) model packages from the [AWS Marketplace](https://aws.amazon.com/marketplace/) with Amazon SageMaker.

Each notebook walks through the full lifecycle of a listing: subscribing to the model package, deploying a real-time endpoint, invoking it with the product's input/output schema, running batch transform jobs, and cleaning up.

## Repository layout

Examples are grouped by **product type**, then by **model** (one folder per marketplace listing). Models of the same product type can have different input/output schemas, so every model folder is fully self-contained — its notebook, sample inputs, and expected outputs live together:

```
<product-type>/                     e.g. speech-to-text, text-to-speech, ocr, llm, translate
└── <model>/                        e.g. saaras-v3.1
    ├── <model>-Model.ipynb         sample notebook for the listing
    ├── README.md                   model overview + I/O schema summary
    └── data/
        ├── input/                  sample payloads used by the notebook
        │   ├── real-time/
        │   ├── streaming/          (only for products with a streaming API)
        │   └── batch/
        └── output/                 expected responses for the sample inputs
            ├── real-time/
            ├── streaming/
            └── batch/
```

## Available examples

| Product type | Model | Notebook |
|---|---|---|
| Speech-to-Text | Saaras v3.1 | [speech-to-text/saaras-v3.1](speech-to-text/saaras-v3.1/saaras-v3.1-speech-to-text-Model.ipynb) |

More product types (text-to-speech, OCR, LLM, translate) will be added as their marketplace listings launch.

## Prerequisites

- An AWS account with an active subscription to the relevant marketplace listing (each notebook links to its listing).
- Amazon SageMaker Studio or a SageMaker Notebook Instance (recommended), with an execution role that has `AmazonSageMakerFullAccess`.
- Service quota for the GPU instance types named in each notebook.

## Support

For questions about a listing or the models, contact Sarvam AI through the support channel listed on the marketplace product page.
