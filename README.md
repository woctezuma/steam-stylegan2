# Steam StyleGAN2

The goal of this [Google Colab](https://colab.research.google.com/) notebook is to catpure the distribution of Steam banners and sample with a StyleGAN2.

## Usage

-   Acquire the data, e.g. as a snapshot called `128x128.zip` in [another of my repositories](https://github.com/woctezuma/download-steam-banners-data),
-   Run `StyleGAN2.ipynb` to train a StyleGAN2.
-   To resume training from a checkpoint, you will have to edit `training/training_loop.py`.

## Results

The dataset consists of 14,035 Steam banners with RGB channels and resized from 300x450 to 128x128 resolution.

> WIP: investigate the out-of-memory crash after the first iteration.

## References

-   [StyleGAN2](https://github.com/NVlabs/stylegan2)
-   [StyleGAN1](https://github.com/NVlabs/stylegan)
-   [Steam-StyleGAN1](https://github.com/woctezuma/steam-stylegan)
-   [Steam-DCGAN](https://github.com/woctezuma/google-colab)
