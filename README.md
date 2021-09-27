# Steam StyleGAN2

![Examples of generated Steam banners](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/generated_banners.jpg)

The goal of this [Google Colab](https://colab.research.google.com/) notebook is to capture the distribution of Steam banners and sample with a StyleGAN2.

## Usage

-   Acquire the data, e.g. as a snapshot called `256x256.zip` in [another of my repositories](https://github.com/woctezuma/download-steam-banners-data),
-   Run [`StyleGAN2_training.ipynb`][StyleGAN2_training] to train a StyleGAN2 model from scratch,
[![Open In Colab][colab-badge]][StyleGAN2_training]
-   Run [`StyleGAN2_image_sampling.ipynb`][StyleGAN2_image_sampling] to generate images with a trained StyleGAN2 model,
[![Open In Colab][colab-badge]][StyleGAN2_image_sampling]
-   To automatically resume training from the latest checkpoint, you will have to use [my fork](https://github.com/woctezuma/stylegan2/tree/google-colab) of StyleGAN2.

For a more thorough analysis of the trained model:
-   Run [`StyleGAN2_metrics.ipynb`][StyleGAN2_metrics] to compute metrics (FID, PPL, etc.),
[![Open In Colab][colab-badge]][StyleGAN2_metrics]
-   Run [`StyleGAN2_latent_discovery.ipynb`][StyleGAN2_latent_discovery] to discover meaningful latent directions,
[![Open In Colab][colab-badge]][StyleGAN2_latent_discovery]
-   Run [`Ganspace_colab_for_steam.ipynb`][Ganspace_colab_for_steam] to specifically use GANSpace for discovery,
[![Open In Colab][colab-badge]][Ganspace_colab_for_steam]

## Data

The dataset consists of 14,035 Steam banners with RGB channels and resized from 300x450 to 256x256 resolution.

Our dataset (14k images) has a similar size to the [FFHQ dataset](https://github.com/NVlabs/ffhq-dataset) (70k images, so about 5 times larger than our dataset).

## Training parameters

The StyleGAN2 model is trained with configuration `e` to accommodate for Google Colab's memory constraints.

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

The last snapshot (`network-snapshot-005000`) can be directly downloaded from the following links for:
-   [TensorFlow][google-drive-last-checkpoint] (`.pkl`),
-   [PyTorch][google-drive-last-checkpoint-pytorch] (`.pt`), thanks to a [model-weight converter][weight-converter].

### Generated Steam banners

The generated images are diverse, in terms of configuration and color palettes. Interestingly, it seems that an anime character (with large eyes, hair, mouth, ear and nose) has been learnt.

A grid of generated Steam banners is shown below:
![Generated Steam banners](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/results/fakes005000.jpg)

For comparison, here is a grid of real Steam banners:
![Real Steam banners](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/results/reals.jpg)

Moreover, a directory of 1000 generated images is shared on Google Drive:
-   [without truncation][google-drive-without-truncation],
-   [with truncation][google-drive-with-truncation].

### Truncation

The term 'truncation' refers to the truncation trick mentioned in Appendix B of [the StyleGAN1 paper](https://arxiv.org/abs/1812.04948):

> If we consider the distribution of training data, it is clear that areas of low density are poorly represented and thus likely to be difficult for the generator to learn. This is a significant open problem in all generative modeling techniques. However, it is known that drawing latent vectors from a truncated [42, 5] or otherwise shrunk [34] sampling space tends to improve average image quality, although some amount of variation is lost.

The influence of truncation can be studied with side-by-side comparison of generated images.
In [this folder][google-drive-truncation-comparison], 25 side-by-side comparisons are provided:
- images to the left are obtained without truncation,
- images to the right are obtained with the same seed and with truncation.

A few examples are shown below:

![Influence of truncation](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/truncation_comparison/seed0001.jpg)
![Influence of truncation](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/truncation_comparison/seed0041.jpg)
![Influence of truncation](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/truncation_comparison/seed0208.jpg)
![Influence of truncation](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/truncation_comparison/seed0322.jpg)
![Influence of truncation](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/truncation_comparison/seed0424.jpg)
![Influence of truncation](https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/truncation_comparison/seed0939.jpg)

### Style Mixing

In the grids below, styles from the first row and first column are mixed.

Similarly to style mixing of faces:
- the pose and geometrical shapes are similar for images in the same column,
- the color palette is similar for images in the same row.

#### Without truncation

<img alt="Style Mixing without truncation" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/style-mixing/00058-style-mixing-example_grid.jpg" width="500">

#### With truncation

With truncation, color palettes are less diverse, which is expected, and seem to be darker.

<img alt="Style Mixing with truncation" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/style-mixing/00059-style-mixing-example_grid.jpg" width="500">

### Projection of real Steam banners to latent space

Real Steam banners can be [projected][projection-github-project] to latent space.
Projections obtained with 23 popular games are shown on [the Wiki](https://github.com/woctezuma/steam-stylegan2/wiki/Projection).
A directory with similar data is also shared on [Google Drive][projection-to-latent-space].

Each row corresponds to a different game.
From left to right, the columns correspond to :
1.  the generated image obtained at the start of the projection,
2.  the generated image obtained at the end of the projection,
3.  the target (real) image.

<img alt="Projected image n°1/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0002-step0200.jpg" width="250"><img alt="Projected image n°5/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0002-step1000.jpg" width="250"><img alt="Target image" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0002-target.jpg" width="250">

<img alt="Projected image n°1/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0003-step0200.jpg" width="250"><img alt="Projected image n°5/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0003-step1000.jpg" width="250"><img alt="Target image" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0003-target.jpg" width="250">

<img alt="Projected image n°1/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0008-step0200.jpg" width="250"><img alt="Projected image n°5/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0008-step1000.jpg" width="250"><img alt="Target image" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0008-target.jpg" width="250">

<img alt="Projected image n°1/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0012-step0200.jpg" width="250"><img alt="Projected image n°5/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0012-step1000.jpg" width="250"><img alt="Target image" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0012-target.jpg" width="250">

<img alt="Projected image n°1/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0017-step0200.jpg" width="250"><img alt="Projected image n°5/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0017-step1000.jpg" width="250"><img alt="Target image" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0017-target.jpg" width="250">

<img alt="Projected image n°1/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0020-step0200.jpg" width="250"><img alt="Projected image n°5/5" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0020-step1000.jpg" width="250"><img alt="Target image" src="https://raw.githubusercontent.com/wiki/woctezuma/steam-stylegan2/img/00060-project-real-images/image0020-target.jpg" width="250">

The title position is usually correct, even though the letters appear as gibberish. The color palette is correct. The appearance of characters is rough. For the last case (Hitman), the projected face is interesting, with the mouth, nose, and what appears to be glasses to reproduce the appearance of eyes in the shade.

## References

-   StyleGAN2:
    -   [StyleGAN2][stylegan2-official-repository] / [StyleGAN2-ADA][stylegan2-ada-official-repository] / [StyleGAN2-ADA-PyTorch][stylegan2-ada-pytorch-repository]
    -   [Projecting images to latent space with StyleGAN2][projection-github-project]
-   StyleGAN:
    -   [StyleGAN1](https://github.com/NVlabs/stylegan)
    -   [Steam-StyleGAN1](https://github.com/woctezuma/steam-stylegan)
-   DCGAN:    
    -   [Steam-DCGAN](https://github.com/woctezuma/google-colab)
-   Useful tools:
    -   My [fork][stylegan2-fork] of StyleGAN2 to project a batch of images, using any projection (original or extended)
    -   A [model-weight converter][weight-converter] from TensorFlow (`.pkl`) to PyTorch (`.pt`), with some instructions [here][weight-converter-instructions]
-   Detailed tutorials:
    -   [This Waifu Does Not Exist](https://www.gwern.net/TWDNE)
    -   [Making Anime Faces With StyleGAN](https://www.gwern.net/Faces)
-   Papers about the unsupervised discovery of latent directions:
    - [Voynov, Andrey, et al. *Unsupervised Discovery of Interpretable Directions in Latent Space*. ICML 2020.][GanLatentDiscovery-paper] **([code][GanLatentDiscovery])**
    - [Härkönen, Erik, et al. *GANSpace: Discovering Interpretable GAN Controls*. arXiv 2020.][ganspace-paper] **([code][ganspace])**
    - [Shen, Yujun, et al. *Closed-Form Factorization of Latent Semantics in GANs*. arXiv 2020.][closed-form-paper] **([code][closed-form])**

<!-- Definitions -->

[stylegan2-official-repository]: <https://github.com/NVlabs/stylegan2>
[stylegan2-ada-official-repository]: <https://github.com/NVlabs/stylegan2-ada>
[stylegan2-ada-pytorch-repository]: <https://github.com/NVlabs/stylegan2-ada-pytorch>

[google-drive-last-checkpoint-pytorch]: <https://drive.google.com/file/d/1-2pWoqvSysNuYS8acfhbU9ql4Si09hyf/view?usp=sharing>
[google-drive-last-checkpoint]: <https://drive.google.com/file/d/1HXczmPE4PMBbhrPOshshnS0KFI4J6jbC/view?usp=sharing>
[google-drive-checkpoints]: <https://drive.google.com/drive/folders/1bf17M5HtqdWYhxhCzo1YCEr9fdS84NO6>
[google-drive-without-truncation]: <https://drive.google.com/open?id=1hCH4y1a9NhXkmgDc6mqnBRdwolsmttg2>
[google-drive-with-truncation]: <https://drive.google.com/open?id=1zvnkPz0mKusrGW6TojFOrZjiqYAKi6sn>
[google-drive-truncation-comparison]: <https://drive.google.com/drive/folders/1itOMCX6h62OWpUCBxmUWcrKh0lSXxSGQ>
[projection-to-latent-space]: <https://drive.google.com/open?id=14Uz3SbOL0G4aLK1AmHye9bJCmOufDdbn>

[projection-github-project]: <https://github.com/woctezuma/stylegan2-projecting-images>
[stylegan2-fork]: <https://github.com/woctezuma/stylegan2/tree/tiled-projector>

[StyleGAN2_training]: <https://colab.research.google.com/github/woctezuma/steam-stylegan2/blob/master/StyleGAN2_training.ipynb>
[StyleGAN2_image_sampling]: <https://colab.research.google.com/github/woctezuma/steam-stylegan2/blob/master/StyleGAN2_image_sampling.ipynb>
[StyleGAN2_metrics]: <https://colab.research.google.com/github/woctezuma/steam-stylegan2/blob/master/StyleGAN2_metrics.ipynb>
[StyleGAN2_latent_discovery]: <https://colab.research.google.com/github/woctezuma/steam-stylegan2/blob/master/StyleGAN2_latent_discovery.ipynb>
[Ganspace_colab_for_steam]: <https://colab.research.google.com/github/woctezuma/steam-stylegan2/blob/ganspace/Ganspace_colab_for_steam.ipynb>

[GanLatentDiscovery-paper]: <https://arxiv.org/abs/2002.03754>
[ganspace-paper]: <https://arxiv.org/abs/2004.02546>
[closed-form-paper]: <https://arxiv.org/abs/2007.06600>

[interfacegan]: <https://github.com/genforce/interfacegan>
[ganspace]: <https://github.com/harskish/ganspace>
[ALAE]: <https://github.com/podgorskiy/ALAE>
[GanLatentDiscovery]: <https://github.com/anvoynov/GanLatentDiscovery>
[closed-form-repository]: <https://github.com/genforce/sefa>
[closed-form]: <https://github.com/rosinality/stylegan2-pytorch#closed-form-factorization-httpsarxivorgabs200706600>

[weight-converter]: <https://github.com/rosinality/stylegan2-pytorch>
[weight-converter-instructions]: <https://github.com/rosinality/stylegan2-pytorch/issues/52>

[colab-badge]: <https://colab.research.google.com/assets/colab-badge.svg>
