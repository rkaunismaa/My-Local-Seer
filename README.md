# My-Local-Seer

This will be my first kick at building a fully local, gpu powered chatbot. Just say no to OpenAI kids ... 

conda activate mls2

Nope! not any more ... 
conda activate mylocalseer

## Tuesday, February 6, 2024

Gonna see if I can resolved the problems with bitsandbytes. Hmm I wonder if I  create a new conda environment but use the pytorch install for cuda 11.8 if it will work...yeah ... gonna torch the current mylocalseer conda environment, then install pytorch for cuda 11.8.0 and see if that works.

Actually, lets just create a new mls2 environment ... 

 1) mamba create -n mls2 python=3.11
 2) mamba activate mls2
 3) mamba install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
 4) mamba install conda-forge::transformers
 5) mamba install conda-forge::jupyterlab
 6) mamba install conda-forge::ipywidgets
 7) mamba install conda-forge::accelerate
 8) mamba install conda-forge::bitsandbytes 

 OK! Nice! This new environment is able to run the TransformersTest.ipynb notebook without errors! So the problems WERE with the version of the cuda toolkit! Good to know!

Now going to test using the OpenAI library to talk to a local llm via LMStudio.

  9) mamba install conda-forge::openai

Yup! This works just fine. 

 

## Monday, February 5, 2024

Created the conda environment for this repo.

 1) mamba create -n mylocalseer python=3.11
 2) mamba activate mylocalseer
 3) mamba install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
 4) mamba install conda-forge::transformers
 5) mamba install conda-forge::jupyterlab

I started with the above installs, and as I tested things, I added these additional libraries:

 6) mamba install conda-forge::ipywidgets
 7) mamba install conda-forge::accelerate
 8) mamba install conda-forge::bitsandbytes   ... (this installs the 11.8.0 cuda toolkit, and scipy 1.12.0)

 

