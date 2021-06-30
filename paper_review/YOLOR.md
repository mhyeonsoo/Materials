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
#### Explicit Deep learning
ex) Transofrmer, Self-attention, Non-local networks
- In here, the authors adapted Transformer
#### Implicit Deep learning
- Implicit neural representations
  - to obtain the parameterized continuous mapping representation of discrete inputs to perform different tasks
- Deep equilibrium models
  - to transform implicit learning into a residual form neural networks.
#### Knowledge modeling

## How implicit knowledge works?


## Implicit knowledge in our unified networks
