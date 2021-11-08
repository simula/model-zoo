---
title: "Submit a model"
date: 2021-09-06T15:05:40+02:00
draft: false
summary: How to save a model and submit it to the Zoo
---

As described in the [Usage]({{< ref usage >}}) section, there are different
supported formats for model save files. As a minimum, the framework-dependent
model files and a software dependency specification must be included. For more
advanced models, a Docker image is useful too. The sections below describe how
to proceed with creating both.

#### *TLDR;*
These things are needed for a model submission: 
 - Model save files, can be any format 
 - `requirements.txt` or similar, specifying exact software dependencies
 - A markdown file describing:
   - Overview of the model and how to use it
   - Description of data preprocessing, ideally with code 
   - GitHub links, if applicable
  - (Optional) a Docker image containing the model and required software

Below we discuss these points in more detail. 


### Create model save files and software specifications list

To create framework-specific save files, call the `save` function (or similar)
on your model like you would normally do, but ensure that everything needed
to recreate it is included. This point is most relevant to PyTorch users (see
below). 

In most cases there is some preprocessing of data involved, which is important
to describe. If the preprocessing step contains few or no parameters, such as
scaling by a fixed number, it is enough to add a code snippet to the model
description. If you have used any of the `Scaler` objects from scikit-learn,
these can be saved to a binary file using `pickle`:

```python
import pickle
with open('my-scalerfile.pkl', 'wb') as f:
    pickle.dump(myscaler, f)
```

If the saved model is composed of several files, or a single file which
is large, please make a `zip` archive:

```shell
# Zip one or several files
zip my-model.zip modelfile1 modelfile2 scalerfile ...
# Zip a directory (recursively)
zip -r my-model.zip my-model-dir
```

To make a list of all required software packages and their versions, use `pip` 
or `conda` as shown below. If your model also relies on software that is not
available from the standard package managers, add detailed installation 
instructions to the model description. In this case one should also strongly
consider to make a Docker image, as described in the next section.

{{< tabbed_code_display "pip;conda;Julia"
  "pip freeze > requirements.txt"
  "conda env export > environment.yml" 
  `pkg> activate .      # Creates a Manifest.toml file
pkg> add <package>   # Installing packages automatically adds them to Manifest.toml` >}}


#### Note on saving PyTorch models and custom TensorFlow functions 

PyTorch allows for saving either the model instance using `pickle`, or the
model's `state_dict`. In either case, the model definition is not included,
so the save file won't be portable. Therefore it is important to document the
model definition either by including it as a python file, or writing it out in
the model description.

The same applies for TensorFlow models using custom layers or loss functions
-- include the code definitions so the custom parts can be loaded on a 
different system.


### Write a good description of how to use the model

With the model save files in place, the Model Zoo entry also needs a good
description of the usage. The description should be formatted in 
[markdown](https://www.markdownguide.org/basic-syntax/) and can typically
be heavily inspired by your project's README.md file, in case documentation
has already been written there. The following points should be considered:
 - Code snippets showing how to load the model, for users unfamiliar with the
 framework used
 - Links to a test dataset that can be used to verify that the model has
 been loaded correctly
 - Code snippets describing how to preprocess the data (**_important_**)

In many cases the model can be documented very well just from code examples.
For inspiration, see the existing models or the
[examples]({{< ref "/categories/examples" >}}).


#### Jupyter notebooks 

A great way to show code examples and let users familiarise themselves with
model inputs and outputs is through Jupyter Notebooks. There is a neat
integration between GitHub and Google Colab that allows for automatically
launching a notebook on GitHub on a Colab instance, without checking out
any code manually: Just append the URL of the notebook file to 
`colab.research.google.com/github`. If the notebook URL is
```
https://github.com/<username>/path/to/notebook.ipynb ,
```
the direct link to a colab instance of this notebook will be
```
https://colab.research.google.com/github/<username>/path/to/notebook.ipynb .
```


### Create a Docker image 

Coming!