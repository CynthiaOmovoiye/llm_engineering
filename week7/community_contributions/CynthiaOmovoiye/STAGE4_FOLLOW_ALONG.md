# Stage 4 Fine-Tuning – Follow-Along Guide

This guide walks you through running **Stage 4** (LoRA fine-tuning) in the Week 7 price-attack notebook on your Mac (Apple M4 Pro, 24 GB).

---

## What You Need

- Project: `llm_engineering` with the `.venv` already set up
- Notebook: `week7_price_attack.ipynb` in this folder
- Goal: Install missing pieces and optionally enable CPU so Stage 4 can run

---

## How bitsandbytes Is Used (QLoRA, Optional)

**bitsandbytes** lets the notebook load the base model in **4-bit** (QLoRA) instead of full float16. That cuts VRAM use so you can run Stage 4 on a smaller GPU (e.g. 8–12 GB).

- **When it’s used:** Only when **CUDA** is available (NVIDIA GPU) **and** `bitsandbytes` is installed. The notebook checks `pp.has_bitsandbytes()` and uses a `BitsAndBytesConfig` (4-bit NF4, bfloat16 compute) so the model loads in ~4-bit; then LoRA is applied on top.
- **When it’s skipped:** On **Mac (MPS)** or **CPU**, or if `bitsandbytes` isn’t installed. The model then loads in normal float16 (or CPU). No change to the rest of the pipeline.
- **To enable on a CUDA machine:**  
  `pip install bitsandbytes`  
  Then run the notebook; Stage 4 will automatically use 4-bit loading when it detects CUDA + bitsandbytes.
- **On Mac:** bitsandbytes is not used (MPS isn’t supported for 4-bit in the same way). The notebook still runs with float16 + MPS or CPU.

---

## Step 1: Install PEFT (Required)

Stage 4 uses **PEFT** for LoRA. Install it in your project venv.

**In Terminal:**

```bash
cd /Users/cynthiaomovoiye/Documents/llm_engineering
.venv/bin/pip install peft
```

**Check it worked:**

```bash
.venv/bin/python -c "import peft; print('peft', peft.__version__)"
```

You should see something like `peft 0.x.x`. If you get an error, fix the install before continuing.

---

## Step 2: Check If Apple GPU (MPS) Is Available

Stage 4 prefers **MPS** (Apple GPU) so training is faster. In Cursor/IDE it often shows as unavailable; in a normal Terminal session it may work.

**In Terminal (using your venv):**

```bash
cd /Users/cynthiaomovoiye/Documents/llm_engineering
.venv/bin/python -c "
import torch
print('MPS available:', torch.backends.mps.is_available())
print('Device for Stage 4 would be:', 'mps' if torch.backends.mps.is_available() else 'cpu')
"
```

- If you see **`MPS available: True`** → Stage 4 can use the Apple GPU. Skip to **Step 4**.
- If you see **`MPS available: False`** → Continue to **Step 3** to allow Stage 4 on CPU.

---

## Step 3: Allow Stage 4 to Run on CPU (If MPS Is False)

By default the notebook **skips** Stage 4 when only CPU is available (to avoid very long runs). If you want to run it anyway on CPU:

1. Open **`week7_price_attack.ipynb`**.
2. Find the **Stage 4** code cell (the one that starts with `# Stage 4: Skip if no GPU/memory...` and has `FINETUNED_PREDICTOR = None`).
3. Locate this line:
   ```python
   if _device == "cpu":
       raise RuntimeError("Stage 4 LoRA skipped on CPU (optional)")
   ```
4. **Either delete those two lines**  
   **or** replace them with:
   ```python
   if _device == "cpu":
       print("Warning: Running Stage 4 on CPU. This will be slow (e.g. 30–60+ min).")
   ```
5. Save the notebook.

You can also reduce load on CPU by using fewer training examples in the same cell. Find:

```python
for it in SUPPORT_POOL[:2000]:
```

and change to e.g. **`SUPPORT_POOL[:500]`** for a quicker test run on CPU.

---

## Step 4: Run the Notebook

**Option A – From Cursor/VS Code**

1. Open `week7_price_attack.ipynb`.
2. Ensure the kernel uses the project’s Python:  
   - Kernel → Change Kernel → Select the interpreter that points to  
     `.../llm_engineering/.venv/bin/python`.
3. Run all cells from the top (or run up to Stage 4, then run the Stage 4 cell).
4. When you reach Stage 4:
   - If **MPS is True**: training will use the Apple GPU and be relatively fast.
   - If **you enabled CPU** in Step 3: training will run on CPU and be slow; let it finish or stop and use fewer examples (e.g. 500) for a test.

**Option B – From Terminal (to try MPS)**

1. In Terminal:
   ```bash
   cd /Users/cynthiaomovoiye/Documents/llm_engineering
   .venv/bin/jupyter notebook
   ```
   (If `jupyter` is missing: `.venv/bin/pip install jupyter` then run the command again.)

2. In the browser, open `week7/community_contributions/CynthiaOmovoiye/week7_price_attack.ipynb`.
3. Run all cells. Sometimes MPS is `True` when the notebook runs from a normal Jupyter session instead of the IDE.

---

## Step 5: What to Expect When Stage 4 Runs

1. **Data:** The cell builds ~2000 (or fewer if you changed it) prompt/completion pairs from your support set.
2. **Model load:** It downloads **Qwen2.5-3B-Instruct** from Hugging Face the first time (a few GB).
3. **Training:** LoRA is attached and the Trainer runs for 1 epoch. You’ll see loss and possibly progress bars.
4. **Validation:** After training, it runs the fine-tuned model on 50 validation items and prints **Stage 4 (LoRA) Val AAE=...**.
5. **Later cells:** The comparison table (Stage 1–5), best-method selection, and frozen exam run as before; if Stage 4 ran, it can be chosen as best.

If anything fails (e.g. out-of-memory on CPU), try:
- Reducing **SUPPORT_POOL[:2000]** to **SUPPORT_POOL[:300]** or **[:500]**.
- In the same cell, reducing **per_device_train_batch_size** from 2 to **1**.

---

## Step 6: Quick Checklist

- [ ] Installed **peft** in `.venv` (Step 1).
- [ ] Checked **MPS** (Step 2); if False, decided whether to allow CPU (Step 3).
- [ ] Opened **week7_price_attack.ipynb** and selected the **.venv** kernel.
- [ ] Ran the notebook; Stage 4 cell either completed or you saw a clear error.
- [ ] (Optional) Ran from Terminal with **Jupyter** to retry with MPS.

---

## If You Prefer Not to Run Stage 4

The rest of the notebook (Stages 1, 2, 3, 5, validation comparison, frozen exam, and charts) does **not** depend on Stage 4. Stage 4 will be skipped and the best method will be chosen from the other stages. You can ignore this guide and still get full results from the other methods.

---

## Summary

| Step | Action |
|------|--------|
| 1 | `pip install peft` in project venv |
| 2 | Check MPS: `torch.backends.mps.is_available()` |
| 3 | If MPS False: remove or relax the “skip on CPU” check in the Stage 4 cell (and optionally use fewer examples) |
| 4 | Run the notebook with the .venv kernel (or via Terminal Jupyter to try MPS) |
| 5 | Let Stage 4 train; check Val AAE and that the rest of the notebook runs |

That’s the full follow-along for getting Stage 4 fine-tuning running on your machine.
