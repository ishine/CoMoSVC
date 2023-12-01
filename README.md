# CoMoSVC

We proposed a consistency model based Singing Voice Conversion system, which is inspired by CoMoSpeech: One-Step Speech and Singing Voice Synthesis via Consistency Model.The details can be found in https://github.com/zhenye234/CoMoSpeech

The paper and codebase of CoMoSVC are still being edited and will be completed as soon as possible.


## Dataset Preparation 

You can refer to different preparation methods based on your needs.

Preparation With Slicing can help you remove the silent parts and slice the audio for stable training.


### 0. Preparation With Slicing

Please place your original dataset in the `dataset_slice` directory.

The original audios can be in any waveformat which should be specified in the command line. You can designate the length of slices you want, the unit of slice_size is milliseconds. The default wavformat and slice_size is mp3 and 10000 respectively.

```shell
python preparation_slice.py -w your_wavformat -s slice_size
```

### 1. Preparation Without Slicing

You can just place the dataset in the `dataset_raw` directory with the following file structure:

```
dataset_raw
├───speaker0
│   ├───xxx1-xxx1.wav
│   ├───...
│   └───Lxx-0xx8.wav
└───speaker1
    ├───xx2-0xxx2.wav
    ├───...
    └───xxx7-xxx007.wav
```


##  Preprocessing

### 1. Resample to 24000Hz and mono

```shell
python preprocessing1_resample.py -n num_process
```
num_process is the number of processes, the default num_process is 5.

### 2. Split the training and validation datasets, and generate configuration files.

```shell
python preprocessing2_flist.py
```

##### diffusion.yaml



### 3. Generate features

You should first download https://drive.google.com/file/d/10LD3sq_zmAibl379yTW5M-LXy2l_xk6h/view and then unzip the zip file by

```shell
unzip m4singer_hifigan.zip
```

the checkpoints of the vocoder will be in the `m4singer_hifigan` directory

Then you should download the checkpoint https://ibm.box.com/s/z1wgl1stco8ffooyatzdwsqn2psd9lrr and the put it in the `Content` directory to extract the content feature.

and then run the command

```shell
python preprocessing3_feature.py -c your_config_file -n num_processes 
```


## Training

### 1. train the teacher model

```shell
python train.py
```
The checkpoints will be saved in the `logs/teacher` directory

### 2. train the como model

#### if you want to adjust the config file, you can duplicate a new config file and modify some parameters.

```shell
python train.py -t False -c Your_new_configfile_path -p The_teacher_model_path 
```

## Inference
