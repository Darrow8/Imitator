# Imitator: Personalized Speech-driven 3D Facial Animation
##### ICCV 2023
![teaser](assets/mountain.jpeg)

[**Imitator**](https://balamuruganthambiraja.github.io/Imitator/)<br/>
[Balamurugan Thambiraja](https://github.com/bala1144),
[Ikhsanul Habibie](),
[Sadegh Aliakbarian](),
[Darren Cosker](),
[Christian Theobalt](),
[Justus Thies]()<br/>

![teaser](assets/teaser.png)
[arXiv](https://arxiv.org/abs/2301.00023) | [BibTeX](#bibtex) | [Project Page](https://balamuruganthambiraja.github.io/Imitator/)


### News
- Accepted to ICCV 2023
- Code uploaded on Sep 27, 2023

## Installation

```
git clone https://github.com/bala1144/Imitator.git
cd Imitator
bash install.sh
```

## Overview of pretrained models

`bash download_asset.sh`

We will update 3 pretrained models
- generalized_model_mbp (Generalized model on VOCAset)
- subj0024_stg02_04seq (FaceTalk_170731_00024_TA personalized model)
- subj0138_stg02_04seq (FaceTalk_170731_00024_TA peneralized model)

### Testing on external audio

To evaluate the external audio, you can use the demo audio on the `assets/demo/`
```
python imitator/test_model_external_audio.py -m pretrained_model/generalized_model_mbp --audio assets/demo/audio1.wav -t FaceTalk_170731_00024_TA -c 2 -r -d 
```
- `-a` path to the audio file
- `-t` specify the subject of the VOCA
- `-c` specify the condition[0,1...7] from VOCA used for testing
- `-r` to render the results as videos
- `-d` to dump the prediction as npy files

### Testing on VOCA
To evaluate the generalized model on VOCA
```
python imitator/test_model_voca.py -m pretrained_model/generalized_model_mbp -r -d
python imitator/test_model_voca.py -m pretrained_model/subj0024_stg02_04seq -r -d
python imitator/test_model_voca.py -m pretrained_model/subj0138_stg02_04seq -r -d
```
The script will run generalized model on the VOCA testset:
- `-c` to specify the config of the dataset; edit the imitator/test/data_cfg.yml to point to your dataset path.

## Data Preparation

### VOCA
Download the [VOCA](https://voca.is.tue.mpg.de/) and prepare using the script from [Faceformer](https://github.com/EvelynFan/FaceFormer)
 `python process_voca_data.py`

## Training models

### Generalized

Train the generalized model on VOCA with
```
python main.py -b cfg/generalized_model/imitator_gen_ab_mbp_vel10.yaml --gpus 0,
```

### Personalized model

First stage of the personalization, we optimize for the Style code using model from generalized model
```
python main.py -b cfg/style_adaption/subj0024_stg01_04seq.yaml --gpus 0,
```

Second stage of the personalization, we optimize for the Style code and Displacements using the model from Stage 01
```
python main.py -b cfg/style_adaption/subj0024_stg02_04seq.yaml --gpus 0,
```

## More

## Shout-outs
Thanks to everyone who makes their code and models available. In particular,

- The architecture of our method is inspired by [Faceformer](https://github.com/EvelynFan/FaceFormer)
- Our repo is built on top of the repos of [Taming_Tranformers](https://github.com/CompVis/taming-transformers) and  [Faceformer](https://github.com/EvelynFan/FaceFormer)
- related works  [Faceformer](https://github.com/EvelynFan/FaceFormer), [codetalker](https://github.com/Doubiiu/CodeTalker), [VOCA](https://github.com/TimoBolkart/voca) and [Meshtalk](https://github.com/facebookresearch/meshtalk)

## BibTeX

```
@InProceedings{Thambiraja_2023_ICCV,
    author    = {Thambiraja, Balamurugan and Habibie, Ikhsanul and Aliakbarian, Sadegh and Cosker, Darren and Theobalt, Christian and Thies, Justus},
    title     = {Imitator: Personalized Speech-driven 3D Facial Animation},
    booktitle = {Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV)},
    month     = {October},
    year      = {2023},
    pages     = {20621-20631}
}
```
