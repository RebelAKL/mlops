# P&ID Symbol Detection: Automated MLOps Pipeline

Turns PDF piping and instrumentation diagrams (P&IDs) into structured JSON by detecting the symbols on each sheet, and automates the full model lifecycle so retraining never depends on someone running scripts by hand.

## What it does

1. **Input:** P&ID schematics as PDFs or images.
2. **Detection:** a YOLOv5 object-detection model finds and classifies the symbols on each diagram.
3. **Output:** structured JSON describing the detected symbols, ready for downstream systems.

## The pipeline

Training is orchestrated as an Azure Machine Learning pipeline (Python SDK v2) with six automated steps:

| Step | What happens |
|---|---|
| 1. Data aggregation | Collects the labelled diagram data needed for training |
| 2. Data split | Splits into training and validation sets |
| 3. Register data asset | Versions the aggregated data as a new Azure ML data asset |
| 4. Train | Trains the symbol-detection model on the registered data |
| 5. Register model | Stores the trained model as a versioned artifact for deployment |
| 6. Tag best model | Tags the best-performing version so it can be identified and deployed |

**Hybrid cloud:** data handling and the model registry run on Azure Machine Learning, while fine-tuning jobs are offloaded to **GCP Vertex AI**.

## Why it matters

- **No manual training runs:** the train → register → tag cycle runs as one pipeline.
- **Reproducible:** every model version links back to the exact data asset it was trained on.
- **Deployment-ready:** the best model is always tagged and easy to find.

## Tech stack

Python · Azure Machine Learning (SDK v2) · GCP Vertex AI · YOLOv5

## Scope

This repo covers training and model lifecycle automation. Model inference is not included.

## Acknowledgements

The Azure ML pipeline structure builds on Microsoft's open-source sample [Azure-Samples/digitization-of-piping-and-instrument-diagrams](https://github.com/Azure-Samples/digitization-of-piping-and-instrument-diagrams), which uses a synthetic P&ID dataset.

See [docs/README.md](./docs/README.md) for detailed documentation.
