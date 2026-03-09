# Week 7 – Price Attack (Cynthia Omovoiye)

Fully **local** and **open-source** price estimation system aimed at beating the tutor’s AAE of **39.85** on the frozen 200-item exam from `ed-donner/items_lite` (first 200 of the test split).

## Constraints

- No OpenRouter, OpenAI, Claude, Gemini, Grok, or paid APIs.
- No Colab.
- Uses only: Hugging Face datasets/models, `transformers`, `sentence-transformers`, PEFT/LoRA, `bitsandbytes` (if available), scikit-learn, XGBoost/CatBoost/LightGBM.

## Environment

- **Python:** 3.10+
- **Run from:** `week7/community-contributions/CynthiaOmovoiye` (or set paths in the notebook).
- **Pricer:** Expects `week7/pricer` on `sys.path` (Item, Tester, evaluate).

### Dependencies

```bash
pip install numpy pandas tqdm scikit-learn datasets sentence-transformers
# Optional: catboost, xgboost, lightgbm (at least one for Stage 3)
# Optional (Stage 4 LoRA): torch, transformers, peft, accelerate
```

- **Apple Silicon:** Prefer MPS; dense embeddings and regressor run on CPU/MPS.
- **CUDA:** Use for Stage 4 LoRA/QLoRA if you run it.

## Setup

1. Clone the repo and `cd week7/community-contributions/CynthiaOmovoiye`.
2. Install dependencies above.
3. Optional: set `HF_TOKEN` for downloading `ed-donner/items_lite` and `McAuley-Lab/Amazon-Reviews-2023` (or use cached data after first run).

## How to run

1. Open `week7_price_attack.ipynb`.
2. Run all cells in order.
   - **Frozen 200:** Loaded once (or from `cache/frozen_eval_200.json` if offline).
   - **Support data:** Rebuilt from Amazon Reviews 2023, cached to `cache/support_rebuilt.json`.
   - **Embeddings:** Cached to `cache/support_emb_bge_small.npy` when using dense retrieval.
3. Validation is used for method selection and blend weight; the **frozen 200-item exam** is run only in the “Frozen exam” section.

## Caches and outputs

| Path | Description |
|------|-------------|
| `cache/frozen_eval_200.json` | Frozen 200 items from items_lite test (no train/retrieve on this). |
| `cache/support_rebuilt.json` | Rebuilt support set (stratified, no exam overlap). |
| `cache/support_emb_bge_small.npy` | Dense embeddings for support (BGE-small). |
| `outputs/frozen_200_predictions.csv` | Final predictions (actual, predicted) for the 200 exam items. |

## Experiment stages

1. **Stage 1:** TF-IDF retrieval + neighbor median (Week 6–style baseline).
2. **Stage 2:** Dense retrieval (BGE-small) + neighbor median.
3. **Stage 3:** Regressor (embedding + metadata + neighbor stats); CatBoost/XGBoost/LightGBM/RF.
4. **Stage 4 (optional):** LoRA fine-tune of Qwen2.5-3B-Instruct; skipped on CPU or if imports fail.
5. **Stage 5:** Hybrid = blend of regressor and neighbor median (weight chosen on validation).

Best method is chosen by **validation AAE**. Only that method is run on the frozen 200; metrics (AAE, MSE, R²) and “beat 39.85?” are reported.

## How final metrics were generated

- **Validation metrics:** From the held-out validation split of the rebuilt support set (never the frozen 200).
- **Frozen exam metrics:** Single run of the best predictor on the first 200 items of `ed-donner/items_lite` test split. No tuning or retrieval from this set.
- **Target:** AAE < 39.85. If not reached, the best validated method is still reported and suggested next steps can be noted in the notebook.
