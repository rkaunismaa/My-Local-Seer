# My-Local-Seer

This will be my first kick at building a fully local, gpu powered chatbot. Just say no to OpenAI kids ... 

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

 

