# Improvement of DART [Assignment 4 11711] by dsiga, rdharani, skandi
# Codes are forked and inspired from DART GitHub 
Re - Implementation for ICLR2022 paper *[Differentiable Prompt Makes Pre-trained Language Models Better Few-shot Learners](https://arxiv.org/pdf/2108.13161.pdf)*.
## The following Environment is required to run the codes
- python@3.6.3, would recommend making a conda environement
- Use `pip install -r requirements.txt` to install dependencies.

## Data source
- 16-shot GLUE dataset from [LM-BFF](https://github.com/princeton-nlp/LM-BFF).
- Generated data consists of 5 random splits (13/21/42/87/100) for a task, each has 16 samples.

## **Data source creation in our environment, please use the following codes in order, inside the DART Directory post cloning our reporsitory**
```bash
  mkdir data
  cd data
  wget https://nlp.cs.princeton.edu/projects/lm-bff/datasets.tar
  tar xvf datasets.tar
  cd ..
  python tools/generate_k_shot_data.py
```

## How to run (remains same as the author's codes, hence below)
- To run across each 5 splits in a task, use `run.py`:
  - In the arguments, `encoder="inner"` is the method proposed in the paper where verbalizers are other trainable tokens; `encoder="manual"` means verbalizers are selected fixed tokens; `encoder="lstm"` refers to the [P-Tuning](https://github.com/THUDM/P-tuning) method.
```bash
$ python run.py -h
usage: run.py [-h] [--encoder {manual,lstm,inner,inner2}] [--task TASK]
              [--num_splits NUM_SPLITS] [--repeat REPEAT] [--load_manual]
              [--extra_mask_rate EXTRA_MASK_RATE]
              [--output_dir_suffix OUTPUT_DIR_SUFFIX]

optional arguments:
  -h, --help            show this help message and exit
  --encoder {manual,lstm,inner,inner2}
  --task TASK
  --num_splits NUM_SPLITS
  --repeat REPEAT
  --load_manual
  --extra_mask_rate EXTRA_MASK_RATE
  --output_dir_suffix OUTPUT_DIR_SUFFIX, -o OUTPUT_DIR_SUFFIX
```
- To train and evaluate on a single split with details recorded, use `inference.py`.
  - Before running, [`task_name`, `label_list`, `prompt_type`] should be configured in the code.
  - `prompt_type="none"` refers to fixed verbalizer training, while `"inner"` refers to the method proposed in the paper. (`"inner2"` is deprecated 2-stage training)
- To find optimal hyper-parameters for each task-split and reproduce our result, please use `sweep.py`:
  - Please refer to documentation for [WandB](https://docs.wandb.ai/) for more details.
  - **❗NOTE: we follow [LM-BFF](https://github.com/princeton-nlp/LM-BFF) to use the corresponding automatic search results with different data split seeds.**



## For example, To run the results for SST-2 on all splits of 16 shot data, you will have to run the below : (in DART Directory)
```bash 
  run.py --task SST-2
```
