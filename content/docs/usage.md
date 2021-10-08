---
title: "Usage"
date: 2021-09-06T14:25:28+02:00
tags: []
category: ['Uncategorised']
draft: false
summary: 'General instructions for how to use models in the Zoo'
---

Loading and successfully running a pre-trained model depends on three prerequisites:
- The model save file(s)
- The correct software versions for loading the save files
- Description of the preprocessing step to be applied to new data

The models provided in the Model Zoo are ideally available in different
formats, catering to different use cases. 

 - *Framework-specific files:* Most flexible for further development, such as
    retraining or transfer learning. Set-up can, however, be complicated and/or
    time-consuming.
 - *Framework-independent files (ONNX format):* Quickest to set up for running
    inference in a minimal environment, but less flexible.
 - *Docker image:* Best for reproducibility -- the model files and all software
    dependencies are packed in a container image that can be run without
    installing anything locally. 


#### Framework-specific files

The framework-specific files are maybe the most natural starting point for
use and development, hence are they always provided for models in the Zoo.
These files are not standardised, and may be of any format, so the description
if the model contains the specific instructions for how to interact with them.
In case it does not, or the instructions are confusing, please complain!
When downloading the files from the Zoo, they may be compressed into a `zip`
or `tar` archive to save disk space, and can be decompressed by

```shell
# If the file ends with .zip
unzip my-model.zip
# If it ends with .tar
tar xf my-model.tar
# If if ends with .tar.gz
tar xzf my-model.tar.gz
```

In many cases the model files need to be accompanied by code definitions,
notably if the model is implemented in PyTorch, or if it is implemented in
Keras/TensorFlow and contains custom layers or losses. Again, the case-specific
instructions will cover this.

The required software packages and their version numbers is specified in a text
file, provided alongside the model files. Since the format of the model files
can and do change between framework versions, it is important to have this
specified exactly, or at least have tested which versions work with the model
file. Depending on your package manager, packages can be installed from the
text file by 

{{< tabbed_code_display "pip;conda" "pip install -r requirements.txt" "conda env create -f environment.yml" >}}


#### Framework-independent files

Intended as the framework to unify all frameworks, the
[Open Neural Network Exchange (ONNX)](onnx.ai) initiative provides a file
format for saving ML models created in any of the major frameworks, such as
TensorFlow, PyTorch, XGBoost and others. It also includes a runtime for easily 
loading and running inference of saved models:

{{< tabbed_code_display "python" `import onnxruntime

session = onnxruntime.InferenceSession('my-model.onnx', None)
input_name = session.get_inputs()[0].name
preds = session.run([], {input_name: my_data})[0]` >}} 

Although a fast and lightweight approach to inference, further development on
ONNX models is limited by the fact that not all frameworks that can export
to ONNX format, support import of the same format (most notably PyTorch);
and imported models necessarily lose the framework-specific components that
make them easy to modify. These downsides are expected to be sorted out over
time, but as for now, we don't encourage ONNX models as basis for developents
such as retraining or fine-tuning. 


#### Container (_Docker_) images

In case you are unfamiliar with software _containerisation_, this essentially
means packing code and all its dependencies in a single file that can be run
isolated from all other software on your system. This means you can run the
code without installing anything. The only requirement is a container
_runtime_, which is provided by [Docker](https://docs.docker.com) (or
other alternatives such as Podman or LXC). Docker is free to download and is
installed on all eX3 nodes. 


| Pros | Cons |
| ---- | ---- |
| Can run any software without installation &nbsp;| Files/downloads are large (GBs) | 
| High reproducibility | Somewhat high learning rate | 
| High portability | | 