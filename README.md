# Game-of-Arrows


## Environment Setup
Note that different library version may affect test results. 

**BERT & GPT2-Base**
```
conda env create -f envirionment_bert_.yml

conda activate bert

pip install torch==1.11.0+cu113 torchvision==0.12.0+cu113 -f https://mirrors.aliyun.com/pytorch-wheels/cu113/
```

## Code Structure
* **results:**  The results of attack and defense. (Please download the *xxx* weights from if you are interested in our results)
    * **train_results:** The results of training from public pretrained models
    * **tsqp_results:** The results of training from public pretrained models corresponding to TSQP methods (TSQP requires to change the training process)
    * **arrowmatch_results**: The results of ARROWMATCH across different datasets.
* **data:** The dataset obtained by the adversary through querying the victim model accounts for 1% of all the training data.
* **utils:** The functions in *ARROWMATCH* & *ARROWCLOAK*
* **evaluation:** evaluate_model.py
* **arrowmatch:** arrowmatch.py
* **blackbox:** blackbox_test.py

## Experiments

**Train public model:**
```
# Normal Train
./train.sh --gpus 2,3 --dataset mnli --output_dir "results/train_results

# TSQP Train
./train_tsqp.sh --gpus 2,3 --dataset mnli --output_dir "results/tsqp_results
```

**Eval model:** 
select the different parameters and run the following scripts to evaluate the results.
```
./evaluate_model.sh --dataset "mnli" --model "bert-base-cased" --obfus "translinkguard" --gpus 0,1              #for ARROWMATCH results
 
 ./evaluate_model.sh --dataset "mnli" --model "bert-base-cased" --obfus "none"  --gpus 0,1                      #for training results

```

**Try ARROWMATCH:** select different datasets and different obfuscation methods to verify the effectiveness of *ARROWMATCH*.

Make sure the training results from public have been saved. 

```
./arrowmatch.sh --gpus 0,1 --dataset mnli --model "bert-base-cased" --obfus translinkguard
```

**Test black-box baseline:** The results of finetuning public model with recovery dataset, which represents the situation that adversary can not get any private information.
```
./blackbox_test.sh --gpus 2,3 --dataset mnli --model "bert-base-cased" --obfus translinkguard
```