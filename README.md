# Amharic ASR Dataset Merge

Notebook that combines four Amharic speech-recognition datasets from the
Hugging Face Hub into a single, deduplicated, re-split corpus, and pushes
the result to the Hub.

**Merged dataset:** [Harbidel/amharic-asr-merged](https://huggingface.co/datasets/Harbidel/amharic-asr-merged)

## Source datasets

- [badrex/amharic-speech](https://huggingface.co/datasets/badrex/amharic-speech)
- [chappM/amharic-bdu-asr](https://huggingface.co/datasets/chappM/amharic-bdu-asr)
- [beimnet777/amharic-asr](https://huggingface.co/datasets/beimnet777/amharic-asr)
- [snapwre/amharic-speech](https://huggingface.co/datasets/snapwre/amharic-speech)

> ⚠️ Before pushing publicly, check each source dataset's license/card to
> confirm redistribution as a merged dataset is allowed. Most CC-BY style
> licenses are fine, but always verify.

## What the notebook does

1. Installs dependencies and logs in to the Hugging Face Hub
2. Inspects each source dataset's raw schema (column/split names differ
   across repos)
3. Standardizes every dataset to a common schema: `audio`, `text`, `source`
4. Concatenates all four into one pool
5. Normalizes Amharic (Ge'ez) text (Unicode NFC, whitespace cleanup) and
   drops empty/broken rows
6. Deduplicates exact-duplicate transcripts across sources
7. Re-splits into `train` / `validation` / `test` (90 / 5 / 5), ignoring the
   original per-source splits to avoid leakage
8. Runs a quick sanity check (listens to a few audio/text pairs) and
   estimates total dataset hours
9. Pushes the merged `DatasetDict` to the Hub and writes a dataset card

## Usage

Open `merge_amharic_asr.ipynb` in Jupyter, JupyterLab, or Google Colab.

```bash
pip install -U datasets huggingface_hub soundfile librosa
```

Run the cells top to bottom:

- The schema-inspection cell (Section 2) prints each source dataset's
  columns/splits — read the output before touching `COLUMN_MAP` in
  Section 3, since column names (e.g. `text` vs `transcript` vs
  `sentence`) differ across repos.
- Section 10 pushes to the Hub. Set `HUB_REPO_ID` to your target repo
  (`your-username/dataset-name`) and set `PRIVATE = True` first if you
  want to review before making it public.
- You'll need a Hugging Face token with **write** access
  ([huggingface.co/settings/tokens](https://huggingface.co/settings/tokens))
  when prompted by `notebook_login()`.

## Requirements

- Python 3.9+
- `datasets`, `huggingface_hub`, `soundfile`, `librosa`

## License

The merge notebook itself has no license restrictions; the resulting
dataset's license follows the terms of the source datasets it combines
(see the warning above).
