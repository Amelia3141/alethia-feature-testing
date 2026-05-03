# Alethia Feature Validation

Validates whether the demographic features identified in the Alethia repo actually encode sex/age information and can steer model outputs.

## Usage (Google Colab)

1. Upload this folder to a GitHub repo
2. Open `validate_features.ipynb` in Colab (click the "Open in Colab" badge or use File > Open notebook > GitHub)
3. Select a **T4 GPU** runtime (Runtime > Change runtime type > T4 GPU)
4. Run all cells

## What it tests

For each patient case, the notebook runs four conditions:

| Condition | What it does |
|-----------|-------------|
| **Plain text** | Demographic info written into the prompt as natural language |
| **Set-value clamp** | Features clamped to their demographic mean activation values |
| **Multiplicative clamp (5x)** | Features multiplied by 5 (the repo's current approach) |
| **Neutral** | No demographic info, no clamping |

If the features genuinely encode demographics, the set-value clamp condition should shift diagnosis probabilities toward the plain-text baseline and away from neutral.

## Key findings being tested

- The repo's multiplicative clamping (`sae_acts *= multiplier`) is a near no-op when features have ~zero activation
- Set-value clamping (setting features to mean activation) is the correct alternative
- The repo's `scorer.py` hooks layer 17 but loads an SAE for layer 20, so clamping never fires; this notebook uses the correct hook name dynamically

## Data

The `data/` folder contains patient case files from the DDXPlus dataset used by the Alethia pipeline.
