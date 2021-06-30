# YOLOr: You Only Learn One Representation: Unified Network for Multiple Tasks

- New version of YOLO series.
- First time of applying transformer backbone at YOLO.
- The authors is implementing SOTA transformer with YOLO which may be released within this year.


## Introduction
- There is no clear definition of 'implicit knowledge'.
- In CNN, implicit knowledge used to mean the features obtained from the deep layers.
- In this paper, the authors call the knowledge that directly **correspond to observation as explicit** knowledge, 
  and **implicit knowledge for the model has nothing to do with observation**.
  
- The authors propose the unified networks to integrate explict and implicit knowledge. (Fig. 2-(c))
  1. A unified network that can accomplish various tasks.
  2. Kernel space alignment, prediction refinement, and multi-task learning has been introduced.
  3. Discussing way of using vector or matrix factorization as a tool to model implicit knowledge.
  4. The author confirmed that this implicit representation can correspond to specific physical characteristic
  5. The proposal achived comparable accuracy as previous SOTA, which is **Scaled-YOLOv4-P7** with 88% decreased inference speed

      ![image](https://user-images.githubusercontent.com/32179857/123898525-71130a00-d9a0-11eb-8096-bc6f862c3242.png)

## Related work
### Explicit Deep learning
ex) Transofrmer, Self-attention, Non-local networks
- In here, the authors adapted Transformer
### Implicit Deep learning
- Implicit neural representations
  - to obtain the parameterized continuous mapping representation of discrete inputs to perform different tasks
- Deep equilibrium models
  - to transform implicit learning into a residual form neural networks.
### Knowledge modeling
- Sparse representation
  - uses exemplar, predifined over complete, or learning dictionary to perform modeling
- Memory networks
  - relies on combining various forms of embedding to form memory
## How implicit knowledge works?
```
The main purpose of this is to conduct a unified network that can effectively train implicit knowledge
The autors focused on,
  1. how to train implicit knowledge and inference it quickly
  2. introduce how implicit knowledge as constant tensor can be applied to various tasks
```
### Manifold space reduction
![image](https://user-images.githubusercontent.com/32179857/123899230-bdab1500-d9a1-11eb-98a4-4b08553f75fd.png)

- if the target categories can be successfully classified by the hyper-plane in the projection space,
  that will be the best outcome
- we can take inner product of the projection vector and implicit representation to achieve the goal 
  of reducing the dimensionality of manifold space and effectively achieving various tasks.

### Kernel space alignment
![image](https://user-images.githubusercontent.com/32179857/123899561-648fb100-d9a2-11eb-86d1-813186f70f58.png)

- Fig4.(a) illustrates an example of kernel space misalignment in multi-task and multi-head NN.
- To deal with this problem, the authors suggest to perform **addition and multiplication of output feature and implicit representation**
  - this enables kernel space to be translated, rotated, and scaled to align each output kernel space. (Fig4. (b))
  - ex) FPN, knowledge distillation, handling zero-shot domain transfer

### More functions
**1. Offset refinement**
  - to make networks to predict the offset of center coordinate
**2. Anchor refinement**
  - to introduce multiplication to automatically search the hyper-parameter set of an anchor (needed for anchor-based OD)
**3. Feature selection**
  - dot-multiplication and concatenation can be used, respectively, to perform multi-task feature selection and to pre-conditions for subsequent calculations




## Implicit knowledge in our unified networks
