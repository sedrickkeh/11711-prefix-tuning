# 11-711 Prefix Tuning Reproduction
This repo contains the code we used to reproduce the paper [Prefix-Tuning: Optimizng Continuous Prompts for Generation](https://arxiv.org/abs/2101.00190).

The main backbone of this code is the Prefix-Tuning repo published by the authors of the original paper.
We cloned this and created our own version [here](https://github.com/sedrickkeh/PrefixTuning).

Some changes we made included the following:
- adjust some file paths
- add processed datasets for low-data experiment settings

## Table of Contents
0. [Quickstart](#0.-quickstart)
1. [Table-to-text Results](#1.-Table-to-text-Results)
2. [Summarization Results](#2.-Summarization-Results)
3. [Low-data Settings](#3.-Low-data-Settings)
4. [Ablation Studies](#4.-Ablation-Studies)


## 0. Quickstart
We recommend looking at the the following notebooks, which cover all the steps, including cloning repos, downloading dependencies, running the training scripts, and doing evaluation. 
- [prefix_tuning_e2e.ipynb](prefix_tuning_e2e.ipynb)
- [prefix_tuning_webnlg.ipynb](prefix_tuning_webnlg.ipynb)
- [prefix_tuning_dart.ipynb](prefix_tuning_dart.ipynb)

--- 
Alternatively, below are instructions to reproduce:
1. Clone the following repos:
```bash
$ git clone https://github.com/sedrickkeh/PrefixTuning.git
$ git clone https://github.com/sedrickkeh/dart.git
$ git clone https://github.com/tuetschek/e2e-metrics.git
```

2. Navigate into the `transformers` folder in `PrefixTuning` and install the following dependencies:
```bash
cd PrefixTuning/transformers
pip install -e .
pip install git+https://github.com/PyTorchLightning/pytorch-lightning
pip install gitpython
pip install rouge_score
pip install sacrebleu
pip install unidecode
```

3. Run experiment code (example below): <br>
```bash
python train_e2e.py --preseqlen 5 --learning_rate 0.00008 --seed 88 --epoch 5
```

4. Run evaluation code 
```bash
bash ./dart/evaluation/run_eval_on_webnlg.sh
```

For 3. and 4. above, note that you may need to modify some of the file paths.

## 1. Table-to-text Results



## 2. Summarization Results
```bash
cd seq2seq

python train_bart.py --mode xsum --preseqlen 200 --do_train yes --fp16 yes --bsz 2 --epoch 15 --gradient_accumulation_step 3 --learning_rate 0.00005 --mid_dim 800
```

## 3. Low-data Settings


## 4. Ablation Studies
