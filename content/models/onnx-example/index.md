---
title: "ONNX Example"
date: 2021-09-08T15:44:31+02:00
tags: ["ONNX", "Fashion-MNIST"]
categories: ["Examples"]
code_repository: "https://github.com/smaeland/model-zoo-onnx-mnist-example"
files:
  requirements:
    display_name: "requirements.txt"
    url: "https://raw.githubusercontent.com/smaeland/model-zoo-onnx-mnist-example/master/requirements.txt"
  model:
    display_name: "onnx-cnn-model.onnx"
    url: "https://github.com/smaeland/model-zoo-onnx-mnist-example/blob/master/onnx-cnn-model.onnx?raw=true"
summary: "Example using ONNX for saving models in a framework-independent format"
draft: False
---


[Open Neural Network Exchange (ONNX)](onnx.ai) is an initiative to provide a
unified format for saving (and running) ML models. It works with PyTorch and
TensorFlow, but also non-neural-network frameworks such as Scikit-Learn and
XGBoost. Here we train a model in PyTorch, and use ONNX to i) run it in
the ONNX runtime, which does not require PyTorch at all, and ii) convert
the model into TensorFlow format.

### Dependencies

```shell
wget {{< param files.requirements.url >}}
pip install -r requirements.txt
```

### Download the model

```shell
wget {{< param files.model.url >}}
```


### Data

The model is trained on the
[Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist) dataset.
Start by downloading it:

```shell
wget http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/train-labels-idx1-ubyte.gz
wget http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/train-images-idx3-ubyte.gz
wget http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/t10k-labels-idx1-ubyte.gz
wget http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/t10k-images-idx3-ubyte.gz
```

Note that these files are gzipped, and does not in fact contain image files,
but can be converted to a more typical format by
```python
import gzip
import numpy as np

def load_mnist(data_file, label_file):

    with gzip.open(label_file, 'rb') as lbpath:
        labels = np.frombuffer(lbpath.read(), dtype=np.uint8, offset=8)
    with gzip.open(data_file, 'rb') as imgpath:
        images = np.frombuffer(imgpath.read(), dtype=np.uint8,offset=16).reshape(len(labels), 784)

    images = np.reshape(images, newshape=(images.shape[0], 1, 28, 28))

    return images, labels

x_test, y_test = load_mnist('t10k-images-idx3-ubyte.gz', 't10k-labels-idx1-ubyte.gz')
class_names = [
    'T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat',
    'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot'
]
```

Preprocessing involves only scaling the pixel values.
```python
def preprocess(data):
    return data / 255.

x_test = preprocess(x_test)
print(x_test.shape)
# (10000, 1, 28, 28)
```


### Predict using the ONNX runtime

The ONNX runtime differs slightly from the usual `model(x_test)` or
`model.predict(x_test)` since one needs to be more verbose about the inputs,
but is optimised for fast inference. Create an `InferenceSession` and identify
the input name and shape:

```python
import onnxruntime

session = onnxruntime.InferenceSession('onnx-cnn-model.onnx', None)

input_node = session.get_inputs()[0]
input_shape = tuple(input_node.shape[1:])
input_name = input_node.name
print(f'input name: {input_name}, shape: {input_shape}')

assert x_test.shape[1:] == input_shape, f'x_test.shape: {x_test.shape} != input_shape: {input_shape}'
```

Note that to run the inference session, the input data needs to be of the
correct type, in this case `float32`. Unlike other frameworks, ONNX does not
convert this automatically.
```python
x_test = x_test.astype(np.float32)
```

Now we can predict on the test set and compute the accuracy:
```python
from sklearn.metrics import accuracy_score

preds = session.run([], {input_name: x_test})[0]

preds_onecold = np.argmax(preds, axis=1)
print('Accuracy:', accuracy_score(y_test, preds_onecold))
# Accuracy: 0.8957
```

### Convert to a TensorFlow model

As mentioned, there are two way of going about this:

#### Convert directly to a new save file, on the command line

The `onnx-tf` package provides a command line tool that takes an ONNX save file
as input, and outputs a TensorFlow model directory:
```shell
onnx-tf convert --infile onnx-cnn-model.onnx --outdir tf-model-converted-from-onnx
```

The output is in SavedModel format (which is *not* the same as what Keras uses),
and must be loaded accordingly:
```python
import tensorflow as tf

model = tf.saved_model.load('tf-model-converted-from-onnx')

# Get the input shape
input_spec = model.signatures['serving_default'].structured_input_signature[1]['input']
input_shape = input_spec.shape
assert x_test.shape[1:] == input_shape[1:], f'x_test.shape[1:]: {x_test.shape[1:]} != input_shape[1:]: {input_shape[1:]}'

# Get the output name 
outputs = model.signatures['serving_default'].structured_outputs
output_name = list(outputs.keys())[0]   
```

With this in place, we can run inference and compute the accuracy again:

```python
infer = model.signatures['serving_default']
    
preds = infer(tf.constant(x_test))[output_name]

preds_onecold = np.argmax(preds, axis=1)
print('Accuracy:', accuracy_score(y_test, preds_onecold))
# Accuracy: 0.8957
```

#### Convert on the fly

Lastly, we can load the ONNX file and convert it to TensorFlow inside a script,
which is rather straight-forward.

```python 

onnx_model = onnx.load('onnx-cnn-model.onnx')
tf_model = prepare(onnx_model)

preds = tf_model.run(x_test).output

preds_onecold = np.argmax(preds, axis=1)
print('Accuracy:', accuracy_score(y_test, preds_onecold))
# Accuracy: 0.8957
```
