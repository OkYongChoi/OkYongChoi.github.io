---
title: "TorchScript & JIT"
toc: true
categories:
  - 
tags:
  - 
---
https://pytorch.org/tutorials/advanced/cpp_export.html
https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning?hl=ko
https://www.youtube.com/watch?v=2awmrMRf0dA

Recently, there is a trend to compile TorchScript using **TensorRT compiler (Torch-TensorRT or Ahead-of-Time (AOT) compiler)** developed by NVIDIA rather than **Just-In-Time (JIT) Compiler**. Alternatively, after converting the pytorch model to **ONNX** format rather than TorchScript, it is optimized through a compiler such as TensorRT compiler.

# Inference Optimization
After training and deploying the model, model inference is required in the service phase.

Model Inference itself also requires a lot of matrix operations. For this, libraries such as MLD and MKL-DNN are used at the CPU level and TensorRT, TorchScript & JIT, etc. are used to optimize the operation at the GPU level.

## TensorRT
 - TensorRT is a SDK that optimizes trained deep learning models to improve inference speed on NVIDIA GPUs. Various combinations such as Torch-TensorRT, TensorFlow-TensorRT, and Onnx-TensorRT are used.
 - Using TensorRT, you can optimize a structure that goes through multiple layers by combining it into a single layer.
 - It is easy to use without much knowledge of CUDA.

## Open Neural Network Exchange (ONNX)
ONNX : A compatible format for deep learning models developed by Facebook (Meta) and Microsoft, which converts a model created in a specific framework into a variety of models.
   - For example, Pytorch -> ONNX conversion -> Tensorflow conversion possible
   - The ONNX model enables efficient inference on many different platforms and hardware. Therefore, when optimizing a pytorch model through tensorrt, it is not converted directly from pytorch to TensorRT, but is converted through the process of Pytorch -> ONNX conversion -> TensorRT.

## TorchScript & Pytorch JIT
Because Pytorch is a framework made as similar to Python as possible, it is insufficient to serve models compared to Tensorflow. To correct this limitation, Meta has introduced **TorchScript** and **Pytorch JIT**, which make it possible to optimize the model and serve it in various environments.   

## lightweight models
Unlike inference optimization, making a lightweight model is achieved during the training process. When a model is trained by reducing the weight, it has been studied a lot recently because it has advantages in terms of GPU resources and memory.

https://rasa.com/blog/compressing-bert-for-faster-prediction-2/

### Pruning
A method of reducing the size of the model by repeating the process of removing nodes with low importance in neural network learning and re-learning.
Through this method, in Deep Compression (2015), the VGG-16 model was reduced by about 49 times, and in Clip-q (2018), the ResNet-50 model was reduced by about 15 times.

### Quantization
How to make a model smaller by lowering its resolution
Increase the computational efficiency by appropriately reducing the precision of the parameter
 - TensorFlow can be lightweight by using TensorRT, an SDK provided by NVIDIA.
 - PyTorch can be lightweight by using APEX provided by NVIDIA.

### Knowledge Distillation (KD)
Wouldn't it be possible to teach the knowledge of a large well-trained deep learning model to a small, untrained student model?

## TorchScript & Pytorch JIT
Usually, to put a model into production, you need two things.
### Portability
Models must be able to be exported to various environments.
It should be able to work not only in the Python interpreter process, but also in a C++ server or mobile /embedded device.
### Performance
Inference latency and throughput should be optimized while maintaining the performance of both.

Pytorch is weak in both aspects of portability and performance, and to solve this, Torchscript converts the code from Eager mode to Script mode.

## Tools to Transition from Eager to Script 
``` torch.jit ```

Eager Mode: Normal python runtime mode, used for prototyping, training, and experimenting.
Script Mode
Converted mode for production deployment
Since it is not executed with the Python Interpreter during runtime, parallel operation and optimization are possible.
So, how to convert from Eager mode to Script mode?

There are two ways to convert a Pytorch model to TorchScript by JIT compiler 
1. Tracing
  - Eager To Script Mode with ```torch.jit.trace()```
  - After understanding the model structure using the input values, the model is recorded through the flow of the input values in the model. 
  - Since the flow is recorded, it is a *statically fixed graph*. 
2. Annotation (script).
  - Eager To Script Mode with ```torch.jit.script()```
  - The JIT compiler directly analyzes the model code and compiles it. 
  - Therefore, *dynamic control flow* (conditional branch, break, etc.) can be used. 
  - However, there is a problem of unsupported python code and type estimation, so it is necessary to check when converting the model.
## Tracing Mode
> Eager To Script Mode with ```torch.jit.trace()```

The tracing method is a method of identifying the model structure using a certain input value (data instance) and recording the model through the flow of this input value in the model. In the case of a model that does not use a lot of conditional statements, it is appropriate to convert it using this method.

Usually, to convert a Pytorch model to Torchscript through tracing, you need to pass an instance of the model along with an example input value to the torch.jit.trace() function.
```python
traced_cell = torch.jit.trace(model, example_inputs)

```
In the case of tracing method, the control flow is cleared because it runs the code, records what happens, and constructs a scriptmodule that does exactly that. To properly represent this module, you can use a script compiler that directly analyzes the python code and converts it into TorchScript.

## Annotation mode
Unlike the tracing mode, this method does not need to transmit data samples. Only an instance of the model needs to be input as input.
```python
class Model(torch.jit.ScriptModule):
    ...
    @torch.jit.script_method #If inherit torch.jit.ScriptModule, add this decorator to methods
    def forward(self, x):
        ...

script_cell = torch.jit.script(model)
```

## Mixing Tracing and Scripting 
https://pytorch.org/docs/stable/jit.html
 - Calling a traced function in script
 - Calling a script function in a traced function

## TorchScript?
TorchScript is used to serialize and optimize models of PyTorch code. You can also use the PyTorch model in C++ by using it.