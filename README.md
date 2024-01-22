# MultiMUC

This is the official repository for the MultiMUC dataset, as presented in our EACL 2024 paper:

> *MultiMUC: Multilingual Template Filling on MUC-4*. William Gantt, Shabnam Behzad, Hannah YougEun An, Yunmo Chen, Aaron Steven White, Benjamin Van Durme, Mahsa Yarmohammadi.

## Getting Started

The repository has the following structure:

- `data.zip`: The MultiMUC data files
- `predictions.zip`: (corrected) test set predictions for IterX, GTT, and ChatGPT
- `iterx/`: IterX source code (needed for computing CEAF-RME scores)
- `scripts/eval.py`: Evaluation script for computing CEAF-REE scores

If you wish only to use the data or model predictions and do not care about running evaluation scripts, then no setup is required (beyond unzipping `data.zip` and `predictions.zip`). However, if you wish to run evaluation on the model predictions, see [Evaluation](#evaluation) below. Beyond the data, predictions, and evaluation scripts provided above, we aim to release instructions for model training and inference soon. Please create an issue if you need this functionality and you find that it has not yet been provided.

### Evaluation

Our paper reports two template filling evaluation metrics, CEAF-REE and CEAF-RME (please see the paper for details on these and on the differences between them). Note that we provide scoring script outputs for both metrics in the same directory as the test set predictions for each (`model`, `setting`, `language`) triple. These can be found in the `predictions/{model}/{setting}/{language}/eval_{model}_ceaf_{rme|ree}.out` files. However, if you wish to replicate these results yourself on our model predictions (or on your own predictions, *mutatis mutandis*), the instructions are given below.

#### CEAF-REE

To compute CEAF-REE scores, you should use the provided `scripts/eval.py` script. This should require only a Python installation (we used 3.9) to run. It can be run from this directory as follows:

```
python scripts.eval.py --pred_file predictions/{model}/{setting}/{language}/test_preds.json --gold_file data/corrected/{language}/test.jsonl [--chinese]
```

where `model` is one of "iterx", "gtt", or "chatgpt"; `setting` is one of "bi", "tgt\_auto", or "tgt\_man"; and `language` is one of "ar", "fa", "ko", "ru", or "zh". The `--chinese` flag should be set only if evaluating on Chinese (`zh`).

#### CEAF-RME

If you wish to compute *CEAF-RME* scores, you will have to initialize the `iterx` git submodule as follows:

```
git submodule init
git submodule update
cd iterx
```

Next, you should follow the environment configuration instructions for the IterX repo that are provided [here](https://github.com/wanmok/iterx#environment-setup). Once you have done so, `cd` into `iterx/`, then compute CEAF-RME scores as follows:

```
PYTHONPATH=./src python scripts/ceaf-scorer.py score --dataset MUC ../../predictions/{model}/{setting}/{language}/test\_preds.json ../../data/corrected/{language}/test.jsonl --file-type GTT 
```

where the possible values for `model`, `setting`, and `language` are the same as above.

## Contact

If you encounter any issues with the data or with replicability of results, please feel free to create an issue or to reach out directly to Will Gantt (wgantt.iv@gmail.com).
