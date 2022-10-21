# Experiments with Stable Diffusion

Added CustomFolderData under ldm/data/simple.py
Added ad_dataset_finetuning under configs/stable-diffusion

To train on a single A100:
Signup for stable diffusion v1.4 from huggingface here https://huggingface.co/CompVis/stable-diffusion-v-1-4-original

Using huggingface cli download model to local cache and obtain checkpoint
```
from huggingface_hub import hf_hub_download
ckpt_path = hf_hub_download(repo_id="CompVis/stable-diffusion-v-1-4-original", filename="sd-v1-4-full
```

Run using the following command
```
python main.py     -t     --base configs/stable-diffusion/ad_dataset_finetune.yaml     --gpus '0,'     --scale_lr False     --num_nodes 1     --check_val_every_n_epoch 10     --finetune_from /home/ubuntu/.cache/huggingface/hub/models--CompVis--stable-diffusion-v-1-4-original/snapshots/0834a76f88354683d3f7ef271cadd28f4757a8cc/sd-v1-4-full-ema.ckpt     data.params.batch_size=2     lightning.trainer.accumulate_grad_batches=1     data.params.validation.params.n_gpus=1
```


--------------------------------------

This repository extends and adds to the [original training repo](https://github.com/pesser/stable-diffusion) for Stable Diffusion.

Currently it adds:

- [Imagic](notebooks/imagic.ipynb)
- [Fine tuning](#fine-tuning)
- [Image variations](#image-variations)
- [Conversion to Huggingface Diffusers](scripts/convert_sd_to_diffusers.py)

## Fine tuning

Makes it easy to fine tune Stable Diffusion on your own dataset. For example generating new Pokemon from text!

[![Open in Replicate](https://img.shields.io/badge/%F0%9F%9A%80-Open%20in%20Replicate-%23fff891)](https://replicate.com/lambdal/text-to-pokemon)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/LambdaLabsML/lambda-diffusers/blob/main/notebooks/pokemon_demo.ipynb)
[![Open in Spaces](https://img.shields.io/badge/%F0%9F%A4%97-Open%20in%20Spaces-orange)](https://huggingface.co/spaces/lambdalabs/text-to-pokemon)

![](assets/pokemontage.jpg)

> Girl with a pearl earring, Cute Obama creature, Donald Trump, Boris Johnson, Totoro, Hello Kitty


For a step by step guide see the [Lambda Labs examples repo](https://github.com/LambdaLabsML/examples).

## Image variations

![](assets/im-vars-thin.jpg)

[![Open Demo](https://img.shields.io/badge/%CE%BB-Open%20Demo-blueviolet)](https://47725.gradio.app/)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1JqNbI_kDq_Gth2MIYdsphgNgyGIJxBgB?usp=sharing)
[![Open in Spaces](https://img.shields.io/badge/%F0%9F%A4%97-Open%20in%20Spaces-orange)](https://huggingface.co/spaces/lambdalabs/stable-diffusion-image-variations)

For more details on the Image Variation model see the [model card](https://huggingface.co/lambdalabs/stable-diffusion-image-conditioned).

- Get access to a Linux machine with a decent NVIDIA GPU (e.g. on [Lambda GPU Cloud](https://lambdalabs.com/service/gpu-cloud))
- Clone this repo
- Make sure PyTorch is installed and then install other requirements: `pip install -r requirements.txt`
- Get model from huggingface hub [lambdalabs/stable-diffusion-image-conditioned](https://huggingface.co/lambdalabs/stable-diffusion-image-conditioned/blob/main/sd-clip-vit-l14-img-embed_ema_only.ckpt)
- Put model in `models/ldm/stable-diffusion-v1/sd-clip-vit-l14-img-embed_ema_only.ckpt`
- Run `scripts/image_variations.py` or `scripts/gradio_variations.py`

All together:
```
git clone https://github.com/justinpinkney/stable-diffusion.git
cd stable-diffusion
mkdir -p models/ldm/stable-diffusion-v1
wget https://huggingface.co/lambdalabs/stable-diffusion-image-conditioned/resolve/main/sd-clip-vit-l14-img-embed_ema_only.ckpt -O models/ldm/stable-diffusion-v1/sd-clip-vit-l14-img-embed_ema_only.ckpt
pip install -r requirements.txt
python scripts/gradio_variations.py
```

Then you should see this:

[![](assets/gradio_variations.jpeg)](https://twitter.com/Buntworthy/status/1565704770056294400)

Trained by [Justin Pinkney](https://www.justinpinkney.com) ([@Buntworthy](https://twitter.com/Buntworthy)) at [Lambda](https://lambdalabs.com/)
