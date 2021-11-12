# 11-711 Prefix Tuning Reproduction
This repo contains the code we used to reproduce the paper [Prefix-Tuning: Optimizng Continuous Prompts for Generation](https://arxiv.org/abs/2101.00190).

The main backbone of this code is the Prefix-Tuning repo published by the authors of the original paper.
We cloned this and created our own version [here](https://github.com/sedrickkeh/PrefixTuning).

Some changes we made included the following:
- adjust some file paths
- add processed datasets for low-data experiment settings

Steps to reproduce:
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

3. Run experiment code (example below)
```bash
python train_e2e.py --preseqlen 5 --learning_rate 0.00008 --seed 88 --epoch 5
```

## 1. Table-to-text Results

## 2. Summarization Results

## 3. Low-data Settings

## 4. Ablation Studies
