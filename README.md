# My-Local-Seer

This will be my first kick at building a fully local, gpu powered chatbot. Just say no to OpenAI kids ... 

As part of this learning experience, I want to try and stay away from LangChain for as long as I can. I think going through the process of building things out yourself will give you a much better idea of why such a tool as LangChain (or LLamaIndex or HayStack or ...) was created.

And this thing I want to build we be able to load local pdfs and query them via some local vector db such as Milvus, Elastic Search, Chroma or FAISS. 

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

OK. So now I want to move onto what is involved with 'reading' a local pdf file ... This exploration will be conducted in the notebook 'playGround\ReadPdf.ipynb'

Actually nope ... giving another look at these local chat thingys I have installed ... GPT4All, Text Generation WebUI and AutoGPT ... 

[Text Generation WebUI](https://github.com/oobabooga/text-generation-webui) is installed to ~/Data2/text_generation-webui. To launch it, terminal into that folder and run 'start_linux.sh'. It uses the gpu, but I cannot see any provision to load local pdf files and query them.

[GPT4All](https://gpt4all.io/index.html) is installed to ~/Data2/gpt4all. To launch it, nautilus into the ~/Data2/gpt4all/bin folder and run 'chat' by double clicking it. I ran this today, and it updated to 2.6.2. I then spun it up. It asks right away which model I want to download. I selected the first model 'Mistral OpenOrca', then clicked Download. It failed, as did all others. I then noticed the Download path was set to '/home/rob/Data2/nomic.ai/GPT4ALL' which did not exist. (I kinda recall torching this folder in the past to free up space on Data2). So I created this folder, then tried to download that model. This time the download worked. 

And yes, GPT4All does exactly what I want it to do. I can load a local pdf file and query it. And it claims to use the gpu. So why not just use that? It is open source and the code is available [here](https://github.com/nomic-ai/gpt4all)

OK .. I can see GPT4ALL use Weaviate for its vector database ... interesting. I wonder why they choose this?

Hmm I am playing with GPT4ALL but it is not using the GPU. ... ok slight settings tweak and now it works. 

[AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) Poking around this repo reveals it may be more to my liking. This stuff is definetly not for end users, but seems targeted at developers.

AutoGPT uses OpenAI and there does not appear to be any way to fork in an alternative, such as LMStudio. It also appears to use chromadb for its vector store. AutoGPT gives me a AutoGen feel ... a way to create agents without coding.

Whelp, AutoGPT is tightly bound to OpenAI, and from the docs it appears to use PineCone as the vector store, not chromadb.

I tried installing it locally and damn is it a confusing install. I think at this point I am gonna just bail on AutoGPT.



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

 

