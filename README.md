# FLAME

This repository contains the FLAME code needed for inference, evaluation, and training.

## 1. Install

```bash
conda create -n flame python=3.10 -y
conda activate flame
pip install -r FLAME/requirements.txt
```

## 2. Prepare checkpoints

Prepare the SAM2 base checkpoint:

```text
FLAME/sam2configs/sam2.1_hiera_base_plus.pt
```

Download the released FLAME checkpoint and matching model config from GitHub Releases:

- Release page: https://github.com/phoenixnir/FLAME/releases/tag/v0.1.0
- FLAME checkpoint: https://github.com/phoenixnir/FLAME/releases/download/v0.1.0/flame_g2_ladmulti_sam2.pth
- Model config: https://github.com/phoenixnir/FLAME/releases/download/v0.1.0/model_params.json

Example:

```bash
mkdir -p FLAME/checkpoints
wget -O FLAME/checkpoints/flame_g2_ladmulti_sam2.pth \
  https://github.com/phoenixnir/FLAME/releases/download/v0.1.0/flame_g2_ladmulti_sam2.pth
wget -O FLAME/checkpoints/model_params.json \
  https://github.com/phoenixnir/FLAME/releases/download/v0.1.0/model_params.json
```

Checkpoint SHA256:

```text
9b6c67e3c35f647a3c9207275b1f1406c36780a2a3e0ee6b0f696d82161991bb  flame_g2_ladmulti_sam2.pth
```

## 3. Prepare data

The default dataset configuration is:

```text
FLAME/configs/datasets_default.json
```

Edit `data_root` and/or each dataset `path` in this file to match your local dataset layout. Relative dataset paths are resolved under `data_root`.

The default config contains training and validation entries for MagicBrush and SID:

```text
data/
  magicbrush_multiedit_train/
  magicbrush_multiedit_val/
  sid_train/
  sid_validation/
```

Each dataset directory should follow the expected FLAME dataset structure used by `LocalForgeryDataset`.

## 4. Train and validate

By default, `train.py` uses `FLAME/configs/datasets_default.json` for both training and validation:

```bash
python FLAME/train.py \
  --dataset_config FLAME/configs/datasets_default.json \
  --sam_config FLAME/sam2configs/sam2.1_hiera_b+.yaml \
  --sam_ckpt FLAME/sam2configs/sam2.1_hiera_base_plus.pt \
  --sam_backend sam2 \
  --img_size 512 \
  --train_force_resize \
  --val_force_resize
```

For all available training options, run:

```bash
python FLAME/train.py --help
```

## 5. Evaluate one dataset directory

```bash
python FLAME/test_dataset.py \
  --dataset /path/to/dataset \
  --model /path/to/flame_checkpoint.pth \
  --config FLAME/checkpoints/model_params.json \
  --output FLAME/outputs/eval_run \
  --device cuda
```

## 6. Run single-image inference

```bash
python FLAME/test.py \
  --image path/to/edited_image.png \
  --source path/to/original_image.png \
  --mask path/to/ground_truth_mask.png \
  --model /path/to/flame_checkpoint.pth \
  --config FLAME/checkpoints/model_params.json \
  --output FLAME/outputs/prediction.png \
  --device cuda
```

`--source` and `--mask` can be omitted if unavailable.
