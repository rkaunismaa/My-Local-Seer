# My-Local-Seer

This will be my first kick at building a fully local, gpu powered chatbot. Just say no to OpenAI kids ... 

As part of this learning experience, I want to try and stay away from LangChain for as long as I can. I think going through the process of building things out yourself will give you a much better idea of why such a tool as LangChain (or LLamaIndex or HayStack or ...) was created.

And this thing I want to build must be able to load local pdfs and query them via some local vector db such as Milvus, Elastic Search, Chroma or FAISS. 

conda activate mls2

Nope! not any more ... 
conda activate mylocalseer

## Thursday, February 15, 2024

Continuing to explore the text splitting part of RAG. The notebook LangChain\5_Levels_Of_Text_Splitting.ipynb uses both LangChain and LLamaIndex. Current version of LLamaIndex on conda-forge is 0.9.48, and on pip it is 0.10.4 which is tagged as 'It is by far the biggest update to our Python package to date' ... https://github.com/run-llama/llama_index/releases. I am going to install the conda version.

* mamba install conda-forge::llama-index

This install down-graded scikit-learn frm 1.4.0 to 1.2.2.

This notebook also required the installation of [Unstructered](https://unstructured-io.github.io/unstructured/)

* pip install "unstructured[pdf]"

Running the above performed a ton of changes! The existing library changes were ...

      Attempting uninstall: Pillow
        Found existing installation: Pillow 9.4.0
        Uninstalling Pillow-9.4.0:
          Successfully uninstalled Pillow-9.4.0
      Attempting uninstall: onnxruntime
        Found existing installation: onnxruntime 1.17.0
        Uninstalling onnxruntime-1.17.0:
          Successfully uninstalled onnxruntime-1.17.0
      Attempting uninstall: pdfminer.six
        Found existing installation: pdfminer.six 20231228
        Uninstalling pdfminer.six-20231228:
          Successfully uninstalled pdfminer.six-20231228

Sigh ... now it is complaining about missing [tesseract](https://github.com/tesseract-ocr/tessdoc) Installation instructions for Ubuntu can be seen [here](https://tesseract-ocr.github.io/tessdoc/Installation.html)

* sudo apt install tesseract-ocr
* sudo apt install libtesseract-dev

(Wow! Those two instructions above automatically came up! Is this Cody at work?? )



## Monday, February 12, 2024

* mamba install conda-forge::einops
* mamba install conda-forge::langchain
* pip install langchain-openai
* mamba install conda-forge::chromadb
* pip install langchainhub

Today I installed LangChain (version 0.1.6) because I want to show an example of how they implement RAG. 

I'm going to try and build a simple chatbot that can answer questions about a local pdf WITHOUT using LangChain.

What!? Installing LangChain wants to downgrade pypdf from the current 4.0.1 to 3.17.4 ... I sure hope it doesn't break anything.

I am really glad I began this process of 'implement yourself' the functionality provided by LangChain for building a RAG application.

## Sunday, February 11, 2024

* The documentation for [pdfminer.six](https://pdfminersix.readthedocs.io/en/latest/index.html) is useless. 
* The documentation for [pypdf](https://pypdf.readthedocs.io/en/stable/index.html) is fantastic!

I kinda doubt there one standard way to extract the text from each chapter that will work for every pdf file. Seems to me there are unique nuances to every pdf that could require code tweaks. But once you have extracted the text the way you want it, storing that text in some external file should only need to be done once. 

Wow, seriously?? This conda environment does not have HuggingFace Sentence Transformers installed??

* mamba install conda-forge::sentence-transformers

## Saturday, February 10, 2024

Today I want to focus on reading a pdf book and creating embeddings for each chapter. I want to use the embeddings to do some basic similarity search.

First pdf library I want to experiment with will be [pypdf](https://pypdf.readthedocs.io/en/latest/)

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

 OK! Nice! This new environment is able to run the TransformersTest.ipynb notebook without errors! So the problems WAS with the version of the cuda toolkit! Good to know!

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

OK. I am going to give AutoGen another quick look ...



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

 

