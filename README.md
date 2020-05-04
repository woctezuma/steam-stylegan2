# Steam StyleGAN2

![Examples of generated Steam banners](https://github.com/woctezuma/steam-stylegan/wiki/steam-stylegan2/img/generated_banners.jpg)

The goal of this [Google Colab](https://colab.research.google.com/) notebook is to catpure the distribution of Steam banners and sample with a StyleGAN2.

## Usage

-   Acquire the data, e.g. as a snapshot called `256x256.zip` in [another of my repositories](https://github.com/woctezuma/download-steam-banners-data),
-   Run `StyleGAN2.ipynb` to train a StyleGAN2.
-   To automatically resume training from the latest checkpoint, you will have to use [my fork](https://github.com/woctezuma/stylegan2/tree/google-colab) of StyleGAN2.

## Data

The dataset consists of 14,035 Steam banners with RGB channels and resized from 300x450 to 256x256 resolution.

Our dataset (14k images) has a similar size to the [FFHQ dataset](https://github.com/NVlabs/ffhq-dataset) (70k images, so about 5 times larger than our dataset).

## Training parameters

The StyleGAN2 model is trained with configuration `e` to accomodate for Google Colab's memory constraints.

Training parameters are arbitrarily chosen, with an inspiration from the values detailed in the StyleGAN2 README for the FFHQ dataset:
-   `--mirror-augment=true`: data augmentation with horitontal mirroring,
-   `--total-kimg=5000`: during training with our dataset, StyleGAN2 will be shown 5 million images, so 5 times fewer images than for FFHQ (25 million images: `--total-kimg=25000`).

With mirroring, the dataset size is about 28k images.
So, the model ends up being trained for 178 epochs.

**Caveat**: there is no good rule-of-thumb! Indeed, the right value would depend on the difficulty of the task (the more complex the task to learn, the longer training is **needed** ; e.g. generating game banners vs. human faces, 256x256 resolution vs. 1024x1024 resolution, etc.), and not solely on the size of the training dataset (the more diverse data is available, the longer training is **possible** without over-fitting the training dataset).

## Results

### Training time

The training is performed for `5,000 kimg`, i.e. 5 million images, which translated into roughly 625 ticks of 8 kimg.
Given that each tick requires 15 minutes of computation, the total training time with 1 GPU on Google Colab is about 6.5 days.

### Model checkpoints

During training, checkpoints of the model are saved every thousand epochs, and shared on [Google Drive][google-drive-checkpoints].

### Generated Steam banners

A grid of generated Steam banners is shown below:
![Generated Steam banners](https://github.com/woctezuma/steam-stylegan/wiki/steam-stylegan2/img/results/fakes005000.jpg)

For comparison, here is a grid of real Steam banners:
![Real Steam banners](https://github.com/woctezuma/steam-stylegan/wiki/steam-stylegan2/img/results/reals.jpg)

Moreover, a directory of 1000 generated images is shared on Google Drive:
-   [without truncation][google-drive-without-truncation],
-   [with truncation][google-drive-with-truncation].

### Style Mixing

#### Without truncation

<img alt="Style Mixing without truncation" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/style-mixing/00058-style-mixing-example_grid.jpg" width="500">

#### With truncation

<img alt="Style Mixing with truncation" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/style-mixing/00059-style-mixing-example_grid.jpg" width="500">

### Projection of real Steam banners to latent space

A directory of projections of real Steam banners to latent space is shared on [Google Drive][projection-to-latent-space].

The real Steam banners are tied to the following appIDs:
-   10, # Counter-Strike
-   105600, # Terraria
-   220, # Half-Life 2
-   240, # Counter-Strike: Source
-   252950, # Rocket League®
-   271590, # Grand Theft Auto V
-   292030, # The Witcher® 3: Wild Hunt
-   304930, # Unturned
-   377160, # Fallout 4
-   400, # Portal
-   4000, # Garry's Mod
-   440, # Team Fortress 2
-   48000, # LIMBO
-   550, # Left 4 Dead 2
-   570, # Dota 2
-   578080, # PLAYERUNKNOWN'S BATTLEGROUNDS
-   60, # Ricochet
-   620, # Portal 2
-   70, # Half-Life
-   72850, # The Elder Scrolls V: Skyrim
-   730, # Counter-Strike: Global Offensive
-   863550, # HITMAN™ 2
-   8870, # BioShock Infinite

TODO

## References

-   StyleGAN2:
    -   [StyleGAN2](https://github.com/NVlabs/stylegan2)
    -   [Projecting images to latent space with StyleGAN2](https://github.com/woctezuma/stylegan2-projecting-images)
-   StyleGAN:
    -   [StyleGAN1](https://github.com/NVlabs/stylegan)
    -   [Steam-StyleGAN1](https://github.com/woctezuma/steam-stylegan)
-   DCGAN:    
    -   [Steam-DCGAN](https://github.com/woctezuma/google-colab)
-   Detailed tutorials:
    -   [This Waifu Does Not Exist](https://www.gwern.net/TWDNE)
    -   [Making Anime Faces With StyleGAN](https://www.gwern.net/Faces)

<!-- Definitions -->

[google-drive-checkpoints]: <https://drive.google.com/drive/folders/1bf17M5HtqdWYhxhCzo1YCEr9fdS84NO6>
[google-drive-without-truncation]: <https://drive.google.com/open?id=1hCH4y1a9NhXkmgDc6mqnBRdwolsmttg2>
[google-drive-with-truncation]: <https://drive.google.com/open?id=1zvnkPz0mKusrGW6TojFOrZjiqYAKi6sn>
[projection-to-latent-space]: <https://drive.google.com/open?id=14Uz3SbOL0G4aLK1AmHye9bJCmOufDdbn>