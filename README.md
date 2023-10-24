## Kurdyukov Alexander (BS21-AI-01, a.kurdyukov@innopolis.university)
### To use this repo (in the way its creator was) it is needed to have an ability to run jupyter notebooks. This can be achieved in several ways:
1) Use [Google Colab](https://colab.research.google.com) and manualy config the kernel 
2) Create [Python virtual environment](https://docs.python.org/3/tutorial/venv.html) and [run local jupyter server](https://www.codecademy.com/article/how-to-use-jupyter-notebooks) with [confugured kernel](https://ipython.readthedocs.io/en/stable/install/kernel_install.html) (Recommended)

### If the user will choose 2nd option:
1) Navigate the folder with cloned repo in CMD or PowerShell 
2) Crete Python virtual environment and activate it `(On this step it is considered that the user has Python and Pip installed on his machine)`
3) The creator of this repo was using Python of version 3.11
4) Execute **```pip install -r requirements.txt```** to install all necessary packages for running code in jupyter notebooks
5) **Please note, that it is needed to install [PyTorch](https://pytorch.org/get-started/locally/) in appropriate for the user configuration manually!**
6) In activated virtual environment execute **```ipython kernel install --name "NAME_OF_THE_KERNEL" --user```** to add virtual environment as Python Kernel and to further use it to run jupyter notebooks
7) Execute **```jupyter notebook```** and wait untill the browser window with navigated repo will be opened
8) Enter user password (if needed) and navigate in the opened window **`/notebooks`** subdirectory and open any wanted notebook
9) In **`Kernels`** tab choose kernel from step **`4)`** 

### **`At this moment user should have an ability to run jupyter notebook's code blocks in any manner`**

### Some comments about the overall structure of the repo:
1) It was decided not to create directories for notebooks with some "code tests" and for python scripts for training/predicting/visualizing. The logic behind it is following: if the user can run code cells in jupyter notebooks in `notebooks/` directory then it is not needed to create any python scripts with argument parsing since it will be almost identical to just deconstructing jupyter notebooks that sounded like waste of time for the creator of this repo.
2) It was decided not to create vizualizatons of the results in the `reports/` directory since all vizuals can be found as output for cells in jupyter notebooks in `notebooks/` directory. If it is needed to see some log info - please read `"Visualization of results of trainig"` section of README.md

### Some comments about the dataset that was used to train models and about the overall structure of notebooks:
1) [This dataset](https://github.com/skoltech-nlp/detox/releases/) in `.tsv` format was used to train all the models. The only one preparatory step that was taken - for training only the part of the dataset where toxicity score of the reference is bigger then the one of the translation was used.
2) The user can find two notebooks with the same preparation of data and similar structure, but with diffrerent language models: [T5-small](https://huggingface.co/t5-small) and [Bart-base-4096](https://huggingface.co/ccdv/lsg-bart-base-4096). 
3) All the models(pretrained) are firstly downloaded, then the appropriate pretraned tokenizers are downloaded
4) After all the downloads whole datasets are tokenized and model objects are created
5) Data collators, functions for calculating metrics: BLEU and Avg. generated sentence lenght are then created
6) Finally, Seq2Seq trainers are configured for both models and the process of training can be started

### And now it is time to speak about hyperparameters for parameters that are considered as "configurable":
1) Partition of the dataset that is taken for a training(it is usefull when it is needed for  "fast check" of the training process)
2) [Models](https://huggingface.co/models), tokenizers, and final model structures(manually created Encoder-Decoder or just the initial model)
3) Name of the model/checkpoint where the trained model state will be saved
4) Hyperparameters of training (in the notebooks they are arguments of Seq2SeqTrainer) such as: **`learning rate`**, **`lr schedulers`**, **`warmup steps`**, **`weights decay rate`** and many more.

### Visualization of results of trainig:
#### If it is the case that the user would like to get some sort of logs and read them in a human manner then it is recommended to check the [appropriate argument](https://huggingface.co/docs/transformers/main_classes/trainer#transformers.Seq2SeqTrainingArguments) to create logs during model training. All logs should be in the [format for visualization](https://discuss.huggingface.co/t/how-to-read-the-logs-created-by-hugging-face-trainer/32279) via [Tensorboard](https://pypi.org/project/tensorboard/) (that is **not** listed in requirements.txt since main logging info of Seq2SeqTrainer was enough for the creator of this repo)

### Prediction making (generation):
#### The user can perform detoxification by either preparing set of sentences(as the dataset for training) to detoxify or just choosing one sentence to be passed(with or without iteration) as argument to `generate` method(or alternative method) of the model that can be loaded from checkpoint or the one that was trained can be used for generation. After the passing of the sentence the model will return output that can be instantly printed.
