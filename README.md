# Fish Scan: Good or Bad to Eat

This project turns on a camera, scans a fish image, identifies the fish, and
classifies whether it is **good** or **bad** to eat based on configurable rules.
It uses the Fishial classification models as the AI backbone.

Reference model repository: https://github.com/fishial/fish-identification

## What you get

- Live camera preview with one-key capture
- Fish classification via TorchScript model
- "Good to eat" / "Bad to eat" decision from a rules file
- Simple CLI for configuration

## Setup

1) Create a virtual environment (recommended) and install requirements:

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

2) Download a Fishial classification model.
   The model pack links are listed in the Fishial repository README.

   Recommended:
   - `classification_rectangle_v9-3.zip` (TorchScript)
   - `labels.json` (already copied into `models/labels.json` for you)

3) Unzip the model (for example `classification_rectangle_v9-3.zip`) and
   move `model.ckpt` into `models/` as `fish_classifier.ckpt`.
   You can delete the extracted folder after moving the file.

4) Use the checkpoint directly:

```
python src/main.py ^
  --model models/fish_classifier.ckpt ^
  --rules config/edibility_rules.json
```

Or, if you have a TorchScript `.ts` model, place it in `models/` and run:

```
models/
  fish_classifier.ts
  labels.json
```

## Run

```
python src/main.py
```

### Web mode

```
python src/main.py --mode web
```

Open http://127.0.0.1:8000 to upload a fish image.

### Keys

- `Space`: capture frame and classify
- `Q`: quit

## Configure edibility rules

Edit `config/edibility_rules.json`:

- `edible`: list of fish class names considered safe
- `risky`: list of fish class names considered risky (flagged as RISK)
- `inedible`: list of fish class names considered unsafe
- `unknown_policy`: `"bad"`, `"good"`, or `"unknown"`

You can keep the list small at first, then expand as needed.

## Common names (optional)

Add common names in `config/common_names.json` using scientific names as keys:

```
{
  "Salmo salar": "Atlantic salmon",
  "Thunnus albacares": "Yellowfin tuna"
}
```

The app will display `Common name (Scientific name)` when available.

## Notes

- This project is a starting point. You should validate results for your use case.
- The camera index can be changed with `--camera-index`.
- Adjust `--threshold` to require higher confidence.

