---
title: "Julia MNIST Example"
date: 2021-09-07T10:30:56+02:00
tags: ['CNN', 'MNIST', ]
categories: ['Examples', 'Computer vision']
code_repository: 'https://github.com/smaeland/model-zoo-julia-mnist-example'
files:
  requirements:
    display_name: 'Manifest.toml'
    url: 'https://raw.githubusercontent.com/smaeland/model-zoo-julia-mnist-example/master/Manifest.toml'
  model:
    display_name: 'cnn-model.bson'
    url: 'https://github.com/smaeland/model-zoo-julia-mnist-example/blob/master/cnn-model.bson?raw=true'
summary: 'Example model in Julia / Flux, for classifying handwritten digits in the MNIST dataset.'
draft: false
---

This short example shows how to download and use a model written in Julia using
the Flux framework. 

### Dependencies

Install dependencies using the Julia package manager:

```shell
wget {{< param files.requirements.url >}}
julia   # press ]
pkg> activate .
```

### Download and import the model

```shell
wget {{< param files.model.url >}}
```

Start Julia and activate the environment, then
```julia
using Flux
import BSON
BSON.@load "cnn-model.bson" model
model
# Chain(Conv((3, 3), 1=>6, relu), MaxPool((2, 2), pad = (0, 0, 0, 0), stride = (2, 2)), Conv((3, 3), 6=>16, relu), flatten, Dense(1936, 10))
```

### Load test data

```julia
import MLDatasets
x_test, y_test = MLDatasets.MNIST.testdata(Float32);
size(x_test)
# (28, 28, 10000)
size(y_test)
# (10000,)
```

### Preprocessing

Images loaded above already have pixel values in the range [0, 1], but is
missing the channel axis, so the only required preprocessing step is to add
that. 

```julia
function preprocess(input_data)
    img_size = size(input_data)[1:2] 
    return reshape(input_data, img_size[1], img_size[2], 1, :)
end

x_test = preprocess(x_test);
```

### Evaluate the model

```julia
using Flux: onehotbatch, onecold

preds = model(x_test);
y_true = onehotbatch(y_test, 0:9);    # Model outputs one-hot encoded values, so use that for y_true too

function accuracy(ŷ, y)
    return sum(onecold(ŷ) .== onecold(y)) / size(y)[end]
end

acc = accuracy(preds, y_true);
println("Accuracy: $acc")
```

