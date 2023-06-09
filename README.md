# SoftVC VITS Singing Voice Conversion

This is an application based on so-vits-svc, so using github on the Internet to install it on my local machine was not successful, so I recorded some steps and instructions for my successful installation on this github.

#### ✨ A fork with a greatly improved interface: [34j/so-vits-svc-fork](https://github.com/34j/so-vits-svc-fork)

#### ✨ A client supports real-time conversion: [w-okada/voice-changer](https://github.com/w-okada/voice-changer)

#### This project is fundamentally different from Vits. Vits is TTS and this project is SVC. TTS cannot be carried out in this project, and Vits cannot carry out SVC, and the two project models are not universal

## Disclaimer

This project is an open source, offline project. Contributors to this project will not provide any form of help to any organization or individual, including but not limited to dataset extraction, dataset processing, computing support, training support, reasoning, etc. Project contributors do not know and cannot know what users are using the project for. Therefore, all AI models and synthesized audio based on the training of this project have nothing to do with the contributors of this project. All problems arising therefrom shall be borne by the users themselves.

This project is run completely offline and cannot collect any user information or obtain user input data. Therefore, contributors to this project are not aware of all user input and models and therefore are not responsible for any user input.

This project is only a framework project, which does not have the function of speech synthesis itself, and all the functions require the user to train the model themselves. Meanwhile, there is no model attached to this project, and any secondary distributed project has nothing to do with the contributors of this project

## 📏 Terms of Use

# Warning: Please solve the authorization problem of the dataset on your own. You shall be solely responsible for any problems caused by the use of non-authorized datasets for training and all consequences thereof.The repository and its maintainer has nothing to do with the consequences!

1. This project is established for academic exchange purposes only and is intended for communication and learning purposes. It is not intended for production environments. 
2. Any videos based on sovits that are published on video platforms must clearly indicate in the description that they are used for voice changing and ${\color{red} specify\ the\ input\ source\ of\ the\ voice\ or\ audio}$, for example, using videos or audios published by others and separating the vocals as input source for conversion, ${\color{red} which\ must\ provide\ clear\ original\ video\ or\ music\ links}$. If your own voice or other synthesized voices from other commercial vocal synthesis software are used as the input source for conversion, you must also explain it in the description.
3. You shall be solely responsible for any infringement problems caused by the input source. When using other commercial vocal synthesis software as input source, please ensure that you comply with the terms of use of the software. Note that many vocal synthesis engines clearly state in their terms of use that they cannot be used for input source conversion.
4. It is forbidden to use the project to engage in illegal activities, religious and political activities. The project developers firmly resist the above activities. If they do not agree with this article, the use of the project is prohibited.
5. Continuing to use this project is deemed as agreeing to the relevant provisions stated in this repository README. This repository README has the obligation to persuade, and is not responsible for any subsequent problems that may arise.
6. If you use this project for any other plan, please contact and inform the author of this repository in advance. Thank you very much.

## 📝 Model Introduction

The singing voice conversion model uses SoftVC content encoder to extract source audio speech features, then the vectors are directly fed into VITS instead of converting to a text based intermediate; thus the pitch and intonations are conserved. Additionally, the vocoder is changed to [NSF HiFiGAN](https://github.com/openvpi/DiffSinger/tree/refactor/modules/nsf_hifigan) to solve the problem of sound interruption.

### 🆕 4.0 Version Update Content

- Feature input is changed to [Content Vec](https://github.com/auspicious3000/contentvec)
- The sampling rate is unified to use 44100Hz
- Due to the change of hop size and other parameters, as well as the streamlining of some model structures, the required GPU memory for inference is **significantly reduced**. The 44kHz GPU memory usage of version 4.0 is even smaller than the 32kHz usage of version 3.0.
- Some code structures have been adjusted
- The dataset creation and training process are consistent with version 3.0, but the model is completely non-universal, and the data set needs to be fully pre-processed again.
- Added an option 1: automatic pitch prediction for vc mode, which means that you don't need to manually enter the pitch key when converting speech, and the pitch of male and female voices can be automatically converted. However, this mode will cause pitch shift when converting songs.
- Added option 2: reduce timbre leakage through k-means clustering scheme, making the timbre more similar to the target timbre.
- Added option 3: Added [NSF-HIFIGAN Enhancer](https://github.com/yxlllc/DDSP-SVC), which has certain sound quality enhancement effect on some models with few train-sets, but has negative effect on well-trained models, so it is closed by default
  
## 💬 About Python Version

I use `conda create -n env_name to install, so it is python 3.10`, and it can work smoothly. If you encounter other problems, try to use `python 3.8.9`

##     Manual installation
Anaconda: 

```
conda create -n so-vits-svc-fork python=3.10 pip
conda activate so-vits-svc-fork
```

Installing without creating a virtual environment may cause a PermissionError if Python is installed in Program Files, etc.

Install this via pip (or your favourite package manager that uses pip):

```
python -m pip install -U pip setuptools wheel
pip install -r requirements.txt
```

${\color{red} Directly\ using\ the\ one\ provided\ by\ github\ does\ not\ work\ properly\ on\ my\ computer!!!}$ 

```pip install -U torch torchaudio --index-url https://download.pytorch.org/whl/cu118```

What this project requires is
torch==1.10.0+cu113
torchaudio==0.10.0+cu113
1.10.0 and 0.10.0 indicate the pytorch version, and cu113 indicates the cuda version 11.3
By analogy, please choose the version that suits you to install.


- torch-1.13.0+cu117 click to download: [torch-1.13.0+cu117-cp310-cp310-win_amd64.whl](https://download.pytorch.org/whl/cu117/torch-1.13.0%2Bcu117-cp310-cp310-win_amd64.whl)
- torchaudio-0.13.0+cu117 click to download: [torchaudio-0.13.0+cu117-cp310-cp310-win_amd64.whl](https://download.pytorch.org/whl/cu117/torchaudio-0.13.0%2Bcu117-cp310-cp310-win_amd64.whl)
- torchvision-0.14.0+cu117 click to download: [torchvision-0.14.0+cu117-cp310-cp310-win_amd64.whl](https://download.pytorch.org/whl/cu117/torchvision-0.14.0%2Bcu117-cp310-cp310-win_amd64.whl)

[FFmpeg](https://ffmpeg.org/): 

I don’t know what’s the use of installing this. Not all github require this installation, but the installation is relatively simple. After downloading a zip file, unzip it at any location, and then add the absolute path of the decompressed `bin folder` to the environment variable.

After the installation is complete, enter `ffmpeg -version` in the cmd console, and if something appears, the installation is successful.

Finally: 
```pip install -U so-vits-svc-fork```

So far all the dependencies are installed, next training needs to prepare the `dataset` and `pre-training model`, the dataset can be collect on youtube, the download format is mp3. Then use `Ultimate Vocal Remover` to separate the vocals and music, and convert the vocals to wav format and divided into 5-10 seconds into a small piece of audio (so-vits-svc provides a py. script). Then download all the pre-trained models and place them in the correct folder to start training. The training time takes my RTX2070 super as an example , each epochs is about 200 seconds, and each epochs is 200 steps, so training for 24 hours is about 80000 steps.

## 📥 Pre-trained Model Files

#### **Required**

- ContentVec: [checkpoint_best_legacy_500.pt](https://ibm.box.com/s/z1wgl1stco8ffooyatzdwsqn2psd9lrr)
  - Place it under the `hubert` directory

#### **Optional(Strongly recommend)**

- Pre-trained model files: [G_0.pth](https://drive.google.com/file/d/1a9TIprkdoMw9a-P5-tWOJSszI6uYcJBP/view?usp=share_link), [D_0.pth](https://drive.google.com/file/d/1zYTYwnqhJibAHm5vZjBgbZE5f7feTw-z/view?usp=share_link)
  - Place them under the `logs/44k` directory

Get them my google drive or in the huggingface.

#### **Optional(Select as Required)**

If you are using the NSF-HIFIGAN enhancer, you will need to download the pre-trained NSF-HIFIGAN model, or not if you do not need it.

- Pre-trained NSF-HIFIGAN Vocoder: [nsf_hifigan_20221211.zip](https://github.com/openvpi/vocoders/releases/download/nsf-hifigan-v1/nsf_hifigan_20221211.zip)
  - Unzip and place the four files under the `pretrain/nsf_hifigan` directory

```shell
# nsf_hifigan
https://github.com/openvpi/vocoders/releases/download/nsf-hifigan-v1/nsf_hifigan_20221211.zip
# Alternatively, you can manually download and place it in the pretrain/nsf_hifigan directory
# URL：https://github.com/openvpi/vocoders/releases/tag/nsf-hifigan-v1
```

## 📊 Dataset Preparation

Simply place the dataset in the `dataset_raw` directory with the following file structure.
Add a folder with singer name in dataset_raw, and put wav audio sources in it, the length of these audio sources should be cut to within 5-10 seconds, otherwise the memory will not be enough.
Because the Vocal Remover on the Internet are only free for one time use, you can download and install [Ultimate Vocal Remover](https://ultimatevocalremover.com/), and the settings are as shown in the figure. 

<img
  src=/image/Ultimate_Vocal_Remover_Setting.png
  alt="Alt text"
  title="Setting"
  style="display: inline-block; margin: 0 auto; width: 50%">
 
```
dataset_raw
├───speaker0
│   ├───xxx_(Vocals).wav
│   ├───...
│   └───xxx_(Vocals).wav
└───speaker1
    ├───xxx_(Vocals).wav
    ├───...
    └───xxx_(Vocals).wav
```

You can customize the speaker name.

```
dataset_raw
└───suijiSUI
    ├───xxx_(Vocals).wav
    ├───...
    └───xxx_(Vocals).wav
```

## 🛠️ Preprocessing

### 0. Slice audio

Slice to `5s - 15s`, a bit longer is no problem. Too long may lead to `torch.cuda.OutOfMemoryError` during training or even pre-processing.

By using [audio-slicer-GUI](https://github.com/flutydeer/audio-slicer) or [audio-slicer-CLI](https://github.com/openvpi/audio-slicer)

In general, only the `Minimum Interval` needs to be adjusted. For statement audio it usually remains default. For singing audio it can be adjusted to `100` or even `50`.

After slicing, delete audio that is too long and too short.

### 1. Resample to 44100Hz and mono

```shell
python resample.py
```

### 2. Automatically split the dataset into training and validation sets, and generate configuration files.

```shell
python preprocess_flist_config.py
```

### 3. Generate hubert and f0

```shell
python preprocess_hubert_f0.py
```

After completing the above steps, the dataset directory will contain the preprocessed data, and the dataset_raw folder can be deleted.

#### You can modify some parameters in the generated config.json

* `keep_ckpts`: Keep the last `keep_ckpts` models during training. Set to `0` will keep them all. Default is `3`.

* `all_in_mem`: Load all dataset to RAM. It can be enabled when the disk IO of some platforms is too low and the system memory is **much larger** than your dataset.

## 🏋️‍♀️ Training

```shell
python train.py -c configs/config.json -m 44k
```

## 🤖 Inference

Use [inference_main.py](https://github.com/svc-develop-team/so-vits-svc/blob/4.0/inference_main.py)

```shell
# Example
python inference_main.py -m "logs/44k/G_30400.pth" -c "configs/config.json" -s "nen" -n "君の知らない物語-src.wav" -t 0
```

Required parameters:
- `-m` | `--model_path`: Path to the model.
- `-c` | `--config_path`: Path to the configuration file.
- `-s` | `--spk_list`: Target speaker name for conversion.
- `-n` | `--clean_names`: A list of wav file names located in the raw folder.
- `-t` | `--trans`: Pitch adjustment, supports positive and negative (semitone) values.

Optional parameters: see the next section
- `-a` | `--auto_predict_f0`: Automatic pitch prediction for voice conversion. Do not enable this when converting songs as it can cause serious pitch issues.
- `-cl` | `--clip`: Voice forced slicing. Set to 0 to turn off(default), duration in seconds.
- `-lg` | `--linear_gradient`: The cross fade length of two audio slices in seconds. If there is a discontinuous voice after forced slicing, you can adjust this value. Otherwise, it is recommended to use. Default 0.
- `-cm` | `--cluster_model_path`: Path to the clustering model. Fill in any value if clustering is not trained.
- `-cr` | `--cluster_infer_ratio`: Proportion of the clustering solution, range 0-1. Fill in 0 if the clustering model is not trained.
- `-fmp` | `--f0_mean_pooling`: Apply mean filter (pooling) to f0, which may improve some hoarse sounds. Enabling this option will reduce inference speed.
- `-eh` | `--enhance`: Whether to use NSF_HIFIGAN enhancer. This option has certain effect on sound quality enhancement for some models with few training sets, but has negative effect on well-trained models, so it is turned off by default.

## 🤔 Optional Settings

If the results from the previous section are satisfactory, or if you didn't understand what is being discussed in the following section, you can skip it, and it won't affect the model usage. (These optional settings have a relatively small impact, and they may have some effect on certain specific data, but in most cases, the difference may not be noticeable.)

### Automatic f0 prediction

During the 4.0 model training, an f0 predictor is also trained, which can be used for automatic pitch prediction during voice conversion. However, if the effect is not good, manual pitch prediction can be used instead. But please do not enable this feature when converting singing voice as it may cause serious pitch shifting!
- Set `auto_predict_f0` to true in inference_main.

### Cluster-based timbre leakage control

Introduction: The clustering scheme can reduce timbre leakage and make the trained model sound more like the target's timbre (although this effect is not very obvious), but using clustering alone will lower the model's clarity (the model may sound unclear). Therefore, this model adopts a fusion method to linearly control the proportion of clustering and non-clustering schemes. In other words, you can manually adjust the ratio between "sounding like the target's timbre" and "being clear and articulate" to find a suitable trade-off point.

The existing steps before clustering do not need to be changed. All you need to do is to train an additional clustering model, which has a relatively low training cost.

- Training process:
  - Train on a machine with good CPU performance. According to my experience, it takes about 4 minutes to train each speaker on a Tencent Cloud machine with 6-core CPU.
  - Execute `python cluster/train_cluster.py`. The output model will be saved in `logs/44k/kmeans_10000.pt`.
- Inference process:
  - Specify `cluster_model_path` in `inference_main.py`.
  - Specify `cluster_infer_ratio` in `inference_main.py`, where `0` means not using clustering at all, `1` means only using clustering, and usually `0.5` is sufficient.

### F0 mean filtering

Introduction: The mean filtering of F0 can effectively reduce the hoarse sound caused by the predicted fluctuation of pitch (the hoarse sound caused by reverb or harmony can not be eliminated temporarily). This function has been greatly improved on some songs. However, some songs are out of tune. If the song appears dumb after reasoning, it can be considered to open.

- Set `f0_mean_pooling` to true in `inference_main.py`

### [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/svc-develop-team/so-vits-svc/blob/4.0/sovits4_for_colab.ipynb) [sovits4_for_colab.ipynb](https://colab.research.google.com/github/svc-develop-team/so-vits-svc/blob/4.0/sovits4_for_colab.ipynb)

**[23/03/16] No longer need to download hubert manually**

**[23/04/14] Support NSF_HIFIGAN enhancer**

## 📤 Exporting to Onnx

Use [onnx_export.py](https://github.com/svc-develop-team/so-vits-svc/blob/4.0/onnx_export.py)

- Create a folder named `checkpoints` and open it
- Create a folder in the `checkpoints` folder as your project folder, naming it after your project, for example `aziplayer`
- Rename your model as `model.pth`, the configuration file as `config.json`, and place them in the `aziplayer` folder you just created
- Modify `"NyaruTaffy"` in `path = "NyaruTaffy"` in [onnx_export.py](https://github.com/svc-develop-team/so-vits-svc/blob/4.0/onnx_export.py) to your project name, `path = "aziplayer"`
- Run [onnx_export.py](https://github.com/svc-develop-team/so-vits-svc/blob/4.0/onnx_export.py)
- Wait for it to finish running. A `model.onnx` will be generated in your project folder, which is the exported model.

### UI support for Onnx models

- [MoeSS](https://github.com/NaruseMioShirakana/MoeSS)
  - [Hubert4.0](https://huggingface.co/NaruseMioShirakana/MoeSS-SUBModel)

Note: For Hubert Onnx models, please use the models provided by MoeSS. Currently, they cannot be exported on their own (Hubert in fairseq has many unsupported operators and things involving constants that can cause errors or result in problems with the input/output shape and results when exported.)

CppDataProcess are some functions to preprocess data used in MoeSS

## ☀️ Previous contributors

For some reason the author deleted the original repository. Because of the negligence of the organization members, the contributor list was cleared because all files were directly reuploaded to this repository at the beginning of the reconstruction of this repository. Now add a previous contributor list to README.md.

*Some members have not listed according to their personal wishes.*

<table>
  <tr>
    <td align="center"><a href="https://github.com/MistEO"><img src="https://avatars.githubusercontent.com/u/18511905?v=4" width="100px;" alt=""/><br /><sub><b>MistEO</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/XiaoMiku01"><img src="https://avatars.githubusercontent.com/u/54094119?v=4" width="100px;" alt=""/><br /><sub><b>XiaoMiku01</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/ForsakenRei"><img src="https://avatars.githubusercontent.com/u/23041178?v=4" width="100px;" alt=""/><br /><sub><b>しぐれ</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/TomoGaSukunai"><img src="https://avatars.githubusercontent.com/u/25863522?v=4" width="100px;" alt=""/><br /><sub><b>TomoGaSukunai</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/Plachtaa"><img src="https://avatars.githubusercontent.com/u/112609742?v=4" width="100px;" alt=""/><br /><sub><b>Plachtaa</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/zdxiaoda"><img src="https://avatars.githubusercontent.com/u/45501959?v=4" width="100px;" alt=""/><br /><sub><b>zd小达</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/Archivoice"><img src="https://avatars.githubusercontent.com/u/107520869?v=4" width="100px;" alt=""/><br /><sub><b>凍聲響世</b></sub></a><br /></td>
  </tr>
</table>

## 📚 Some legal provisions for reference

#### Any country, region, organization, or individual using this project must comply with the following laws.

#### 《民法典》

##### 第一千零一十九条 

任何组织或者个人不得以丑化、污损，或者利用信息技术手段伪造等方式侵害他人的肖像权。未经肖像权人同意，不得制作、使用、公开肖像权人的肖像，但是法律另有规定的除外。未经肖像权人同意，肖像作品权利人不得以发表、复制、发行、出租、展览等方式使用或者公开肖像权人的肖像。对自然人声音的保护，参照适用肖像权保护的有关规定。

#####  第一千零二十四条 

【名誉权】民事主体享有名誉权。任何组织或者个人不得以侮辱、诽谤等方式侵害他人的名誉权。  

#####  第一千零二十七条

【作品侵害名誉权】行为人发表的文学、艺术作品以真人真事或者特定人为描述对象，含有侮辱、诽谤内容，侵害他人名誉权的，受害人有权依法请求该行为人承担民事责任。行为人发表的文学、艺术作品不以特定人为描述对象，仅其中的情节与该特定人的情况相似的，不承担民事责任。  

#### 《[中华人民共和国宪法](http://www.gov.cn/guoqing/2018-03/22/content_5276318.htm)》

#### 《[中华人民共和国刑法](http://gongbao.court.gov.cn/Details/f8e30d0689b23f57bfc782d21035c3.html?sw=%E4%B8%AD%E5%8D%8E%E4%BA%BA%E6%B0%91%E5%85%B1%E5%92%8C%E5%9B%BD%E5%88%91%E6%B3%95)》

#### 《[中华人民共和国民法典](http://gongbao.court.gov.cn/Details/51eb6750b8361f79be8f90d09bc202.html)》

