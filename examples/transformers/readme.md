# Examples for Transformers

## Pruning ViT from [Timm](https://github.com/huggingface/pytorch-image-models)

### Data
Please prepare the ImageNet-1K dataset as follows and modify the data root in the script.
```
imagenet/
  train/
    n01440764/
      n01440764_10026.JPEG
      ...
    n01773157/
    n02051845/
    ...
  val/
    n01440764/
      ILSVRC2012_val_00000293.JPEG
      ...
    n01773157/
    n02051845/
```

### Pruning
```bash
bash scripts/prune_timm_vit_b_16_taylor_uniform.sh
```

```
...
----------------------------------------
Summary:
Base MACs: 17.59 G, Pruned MACs: 4.61 G
Base Params: 86.57 M, Pruned Params: 22.05 M
Base Loss: 0.7774, Pruned Loss: 7.1335
Base Accuracy: 0.8108, Pruned Accuracy: 0.0033
Saving the pruned model to output/pruned/vit_base_patch16_224_pruned_taylor_uniform.pth...
```

### Finetuning
```bash
bash scripts/finetune_timm_vit_b_16_taylor_uniform.sh
```
Some results:

| | Vit-B/16 (Timm) |	ViT_B/32 (Timm) | Group L1 (Uniform) | Group Taylor (Uniform) | Group Taylor (Bottleneck) | Group Hessian (Uniform) |
| :-- | :--: | :--: | :--: | :--: | :--: |
| **#Params** | 86.57 M		|  	88.22 M | 22.05 M | 22.05 M | 22.8 M | 22.05 M |
| **MACs** | 17.59 G		| 4.41 G |  4.61 G	| 4.61 G | 4.23 G | 4.61 G |
| **Acc @ Ep 300** | 81.08		| 72.26 | 79.20	| 80.21 | 79.11 |     |
| **Acc @ Ep 50** | -	| - |  69.24	| 71.93 | 71.54 | |

* Uniform - The same pruning ratio for all layers.
* Bottleneck - Only prune the internal dimensions of Attention & FFN.

<div align="center">
<img src="https://github.com/VainF/Torch-Pruning/assets/18592211/24de19ff-60aa-4402-94e3-527670ffb55e" width="80%"></img>
</div>

### Which pruner should be used for ViT pruning?

* Pruning a Vision Transformer pre-trained on ImageNet-1K without fine-tuning.
<div align="center">
<img src="https://github.com/VainF/Torch-Pruning/assets/18592211/6f99aa90-259d-41e8-902a-35675a9c9d90" width="45%"></img>
<img src="https://github.com/VainF/Torch-Pruning/assets/18592211/11473499-d28a-434b-a8d6-1a53c4b3b7c0" width="45%"></img>
</div>


## Pruning Other Transformers

### ViT from [HF Transformers](https://huggingface.co/docs/transformers/index)
```bash
python prune_hf_vit.py
```
```
...
Base MACs: 16.848735 G, Pruned MACs: 4.241336 G
Base Params: 86.567656 M, Pruned Params: 22.050664 M
```

### Swin Transformers from [HF Transformers](https://huggingface.co/docs/transformers/index)
```bash
python prune_hf_swin.py
```
```
...
Base MACs: 4.350805 G, Pruned MACs: 1.438424 G
Base Params: 28.288354 M, Pruned Params: 9.462802 M
```

### Bert from [HF Transformers](https://huggingface.co/docs/transformers/index)
```bash
python prune_hf_bert.py
```
```
...
Base MACs: 680.150784 M, Pruned MACs: 170.206464 M
Base Params: 109.482240 M, Pruned Params: 33.507840 M
```

## Ackowledgement

The training code was adpated from [Torchvision Reference](https://github.com/pytorch/vision/tree/main/references/classification).


