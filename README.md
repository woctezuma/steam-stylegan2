# Steam StyleGAN2

The goal of this [Google Colab](https://colab.research.google.com/) notebook is to catpure the distribution of Steam banners and sample with a StyleGAN2.

## Usage

-   Acquire the data, e.g. as a snapshot called `128x128.zip` in [another of my repositories](https://github.com/woctezuma/download-steam-banners-data),
-   Run `StyleGAN2.ipynb` to train a StyleGAN2.
-   To resume training from a checkpoint, you will have to edit `training/training_loop.py`.

## Results

The dataset consists of 14,035 Steam banners with RGB channels and resized from 300x450 to 128x128 resolution.

> Work-in-progress

## References

-   StyleGAN2:
    -   [StyleGAN2](https://github.com/NVlabs/stylegan2)
    -   [Projecting images to latent space with StyleGAN2](https://github.com/woctezuma/stylegan2-projecting-images)
    -   [My fork](https://github.com/woctezuma/stylegan2)
    -   Interesting forks:
        - TODO
-   StyleGAN:
    -   [StyleGAN1](https://github.com/NVlabs/stylegan)
    -   [Steam-StyleGAN1](https://github.com/woctezuma/steam-stylegan)
-   DCGAN:    
    -   [Steam-DCGAN](https://github.com/woctezuma/google-colab)
