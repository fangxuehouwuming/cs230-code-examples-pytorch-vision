# Named Entity Recognition with Tensorflow

Note : all scripts must be run in `tensorflow/nlp`.

## Requirements

We recommend using python3 and a virtual env. See instructions [here](https://cs230-stanford.github.io/project-starter-code.html).

```
virtualenv -p python3 .env
source .env/bin/activate
pip install -r requirements.txt
```

When you're done working on the project, deactivate the virtual environment with `deactivate`.

## Task

Given a sentence, give a tag to each word ([wikipedia](https://en.wikipedia.org/wiki/Named-entity_recognition))

```
John   lives in New   York
B-PER  O     O  B-LOC I-LOC
```

## [optional] Download the Kaggle dataset (~5 min)

We provide a small subset of the kaggle dataset (30 sentences) for testing in `data/small` but you are encouraged to download the original version on the [Kaggle](https://www.kaggle.com/abhinavwalia95/entity-annotated-corpus/data) website.

1. __Download the dataset__ `ner_dataset.csv` on [Kaggle](https://www.kaggle.com/abhinavwalia95/entity-annotated-corpus/data) and save it under the `nlp/data/kaggle` directory. Make sure you download the simple version `ner_dataset.csv` and NOT the full version `ner.csv`.

2. __Build the dataset__ Run the following script

```
python build_kaggle_dataset.py
```

It will extract the sentences and labels from the dataset, split it into train / test / dev and save it in a convenient format for our model.

*Debug* If you get some errors, check that you downloaded the right file and saved it in the right directory. If you have issues with encoding, try running the script with python 2.7.

3. In the next section, change `data/small` by `data/kaggle`


## Quickstart (~15 min)

1. __Build__ vocabularies and parameters for your dataset by running

```
python build_vocab.py --data_dir="data/small"
```

It will write vocabulary files `words.txt` and `tags.txt` containing the words and tags in the dataset. It will also save a `dataset_params.json` with some extra information.

2. __Your first experiment__ We created a `test` directory for you under the `experiments` directory. It countains a file `params.json` which sets the parameters for the experiment. It looks like

```json
{
    "learning_rate": 1e-3,
    "batch_size": 5,
    "num_epochs": 2,
}
```

For every new experiment, you will need to create a new directory under `experiments` with a `params.json` file.

3. __Train__ your experiment. Simply run

```
python train.py --data_dir="data/small" --model_dir="experiments/test"
```


It will instantiate a model and train it on the training set following the parameters specified in `params.json`. It will also evaluate some metrics on the development set.

4. __Your first hyperparameters search__ We created a new directory `learning_rate` in `experiments` for you. Now, run

```
python search_hyperparams.py --parent_dir="experiments/learning_rate"
```

It will train and evaluate a model with different values of learning rate defined in `search_hyperparams.py` and create a new directory for each experiment under `experiments/learning_rate/`.

5. __Display the results__ of the hyperparameters search in a nice format

```
python synthesize_results.py --parent_dir="experiments/learning_rate"
```

6. __Evaluation on the test set__ Once you've run many experiments and selected your best model and hyperparameters based on the performance on the development set, you can finally evaluate the performance of your model on the test set. Run

```
python evaluate.py --data_dir="data/small" --model_dir="experiments/test"
```