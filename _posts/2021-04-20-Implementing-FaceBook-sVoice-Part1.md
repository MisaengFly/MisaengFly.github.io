---
layout: post
title:  "Training FaceBook sVoice - Part 1"
tags:
  - Implementation
  - Speech Separation
  - sVoice
hero: https://source.unsplash.com/collection/430471/
overlay: red
published: true

---
sVoice is a recently introduced state-of-the-art speech separation technique by FaceBook. The technique, introduced in
the paper "Voice Separation with an Unknown Number of Multiple Speakers", successfully separates voices (or speeches)
of multiple speakers in a single audio sequence.

## Git Clone
This post will briefly run through training the open source sVoice model, and introduce some of the
challenges that I faced along the way. The sVoice github repository is available at:
https://github.com/facebookresearch/svoice.

Before anything else, let me mention that all of the following step were processed on a remote computing server.

To start off, clone the sVoice repository. Cloning the repository will create a new folder named 'svoice'.
~~~js
$git clone https://github.com/facebookresearch/svoice.git
$cd svoice
~~~

## Environment Setup
Now, let's install the dependencies following the instructions available on the sVoice repository.
~~~js
$pip install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html
$pip install -r requirements.txt  
~~~

Install the `NVIDIA CUDA toolkit` to make use of your gpu resources. Follow the developer document available (https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).
Using a Linux (Ubuntu 16.04) environment, the following are the commands I ran to install the CUDA toolkit.
~~~js
$wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-ubuntu1604.pin
$sudo mv cuda-ubuntu1604.pin /etc/apt/preferences.d/cuda-repository-pin-600
$sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
$sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/ /"
$sudo apt-get update
$sudo apt-get -y install cuda
~~~

This should do for setting up the environment for sVoice. 

## Data Preparation
The next part is to prepare a training dataset. You may choose to use your own dataset, but I will train my sVoice model
with an opensource audio dataset LibriMix. If you want a quick toy project with minimal dataset, sVoice also offers a
toy dataset.
~~~js
./make_debug.sh
~~~

####LibriMix
LibriMix is an dataset for speaker diarization with noise that combines LibriSpeech and
WHAM datasets. You can find more info at: https://github.com/JorisCos/LibriMix. <br>

To generate the LibriMix dataset, first clone the above repository and install `SoX`.
~~~js
$git clone https://github.com/JorisCos/LibriMix
$sudo apt-get install sox
~~~

Then you can adjust settings for your dataset by editing `generate_librimix.sh`. You may adjust the number of sources, 
sample rate, noise, and mode (check repository for more detailed information) at the bottom of `generate_librimix.sh`. 
~~~js
$cd LibriMix 
$vi ./generate_librimix.sh
~~~

LibriMix on default setting is a quite massive in size (around 750GB in total), so you may need to make some changes if
you are looking for something simple. I changed my settings to 2 speakers, sample rate of 8 KHz, min (the mixture ends when the 
shortest source ends), and mix_both (utterances + noise).

If you are ready to generate your dataset, run `generate_librimix.sh`.
~~~js
$./generate_librimix.sh storage_dir
~~~
If you are having permission issues running the file, try changing the file permissions before running the file.
~~~js
$chmod 755 ./generate_librimix.sh
~~~

Create a new directory called `librimix` for your dataset under `svoice/dataset`. Under `svoice/dataset/librimix/`, 
Create three new directories `mix`, `s1`, `s2` and place your `mix_both`, `s1`, and `s2` data files under each directories.   

For the dataset, we also need to generate a relevant JSON file in `svoice/egs`. Create a new directory `librimix` and 
run `svoice.data.audio` to generate the JSON files. 
~~~js
!python -m svoice.data.audio dataset/librimix/mix > egs/librimix/mix.json
!python -m svoice.data.audio dataset/librimix/s1 > egs/librimix/s1.json
!python -m svoice.data.audio dataset/librimix/s2 > egs/librimix/s2.json
~~~

Next, go to `svoice/conf/dset/` and create a new yaml file for your dataset. Name your file `librimix.yaml`.
Assign paths to your dataset and the JSON file. For my example, it should look like:
~~~js
# @package dset
train: egs/librimix
valid: egs/librimix
test: egs/librimix
mix_json: egs/librimix/mix.json
mix_dir:
~~~

We have completed data preparation before training the sVoice model. On our next post, we will be looking at how to actually 
begin training the model and solve issues that I encountered along the way.

