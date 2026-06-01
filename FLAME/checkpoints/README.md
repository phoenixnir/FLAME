# Checkpoints

The FLAME checkpoint is distributed through GitHub Releases rather than committed to the repository.

Download page:

```text
https://github.com/phoenixnir/FLAME/releases/tag/v0.1.0
```

Direct downloads:

```bash
wget -O FLAME/checkpoints/flame_g2_ladmulti_sam2.pth \
  https://github.com/phoenixnir/FLAME/releases/download/v0.1.0/flame_g2_ladmulti_sam2.pth
wget -O FLAME/checkpoints/model_params.json \
  https://github.com/phoenixnir/FLAME/releases/download/v0.1.0/model_params.json
```

Checkpoint SHA256:

```text
9b6c67e3c35f647a3c9207275b1f1406c36780a2a3e0ee6b0f696d82161991bb  flame_g2_ladmulti_sam2.pth
```

The SAM2 base checkpoint should be placed at:

```text
FLAME/sam2configs/sam2.1_hiera_base_plus.pt
```

Alternatively, pass your local SAM2 checkpoint path with `--sam_ckpt` when training.
