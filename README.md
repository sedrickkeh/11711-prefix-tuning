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
There are three datasets here, namely [E2E](prefix_tuning_e2e.ipynb), [WebNLG](prefix_tuning_webnlg.ipynb), and [DART](prefix_tuning_dart.ipynb).

**1. E2E** <br>

The script below automatically does evaluation after it trains the model.
```bash
python train_e2e.py --preseqlen 5 --learning_rate 0.00007 --seed 22 --epoch 5 --notes earlystop
```

**2. WebNLG** <br>

Training
```bash
python train_e2e.py --mode webnlg --preseqlen 5 --learning_rate 0.00005 --bsz 5 --seed 222 --epoch 5 --notes earlystop
```
Evaluation
```bash
bash ./dart/evaluation/run_eval_on_webnlg.sh
```

**3. DART** <br>

Training
```bash
python train_e2e.py --mode triples --preseqlen 20 --seed 9 --bsz 5 --epoch 5 --learning_rate 0.00008
```
Evaluation is quite long and may require installing some libraries. Please refer to the [DART notebook](prefix_tuning_dart.ipynb).


## 2. Summarization Results
```bash
cd seq2seq

python train_bart.py --mode xsum --preseqlen 200 --do_train yes --fp16 yes --bsz 2 --epoch 15 --gradient_accumulation_step 3 --learning_rate 0.00005 --mid_dim 800
```

## 3. Low-data Settings
Before running these experiments, first construct the low-data datasets using [this script](https://github.com/sedrickkeh/PrefixTuning/blob/cleaned/data/e2e_data/sample.py). <br>

Dataset size 50
```bash
python train_e2e.py --preseqlen 5 --learning_rate 8e-5 --seed 88 --bsz 10 --lowdata_token 'table-to-text-restaurant:' --epoch 100 --warmup_steps 300 --notes earlystoplowdata_88_50
```
Dataset size 100
```bash
python train_e2e.py --preseqlen 5 --learning_rate 7e-5 --seed 88 --bsz 10 --lowdata_token 'table-to-text-restaurant:' --epoch 100 --warmup_steps 100 --notes earlystoplowdata_88_100
```

Experiments were done for dataset sizes 50, 100, 200, and 500. Scripts for dataset sizes 200 and 500 are analogous to the ones above. Exact hyperparameters can be found in the appendix of our submitted report.

## 4. Ablation Studies
We conduct two ablation studies:
1. Prefix Length <br>
This builds on the experiments for DART.
```bash
python train_e2e.py --mode triples --preseqlen (prefix_length) --seed 9 --bsz 5 --epoch 5 --learning_rate 8e-5
```

2. Prefix Initialization <br>
This builds on the experiments for low-data E2E settings.
```bash
python train_e2e.py --preseqlen 5 --lowdata_token (insert_initialization_here) --learning_rate 7e-5 --seed 88 --bsz 10 --epoch 100 --warmup_steps 100 --notes earlystoplowdata_88_500
```
