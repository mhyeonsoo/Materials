# Learning by Association - A versatile semi-supervised training method for neural networks

[CVPR 2017] 에서 발표된 논문으로, labeling 된 데이터셋의 획득이 난해한 경우에 적용해볼 수 있는 Method 인 것 같아서 리뷰해 보았습니다.

---

### Basic Concept

사람이 어떠한 물체를 보고 그 물체를 식별한다고 할 때, 기존에 경험했던 같은 class의 사물들에 대한 '기억' 과 '연관' 으로 물체를 식별해내는 경우가 있습니다. 이러한 것을 심리학 용어로 'Learning by Association' 이라 하는데,  
이것을 응용한 것입니다.

<img src="https://3.bp.blogspot.com/-rg_FZZ_vnK4/WVhuVNoYNCI/AAAAAAAAB5Q/jXcUP3kAPbscVhOBNX2u0XdHb-fnQhcBgCK4BGAYYCw/s1600/lba_1.png" width="500" height="300" />

Dataset에 대해, A 를 Labeled Data, B 를 Unlabeled Data 라고 하였을 때, A와 B로 부터 Feature vector를 추출하고, 두 feature vector의 유사도로 Unlabeled data의 class를 찾아주는 알고리즘입니다.  
유사도로 사용되는 것은 내적(dot product)을 채택하였습니다.

{{formula fontSize="small"}}
Ssim(A_i,B_i) = A_i \cdot B_i (where  A_i,  B_i  are  extracted  feature  vectors)
{{/formula}}

더불어 해당 paper에서는 어떠한 Data가 들어왔을 때 embedding을 통해 feature vector를 만들고, 이러한 embedding space 에서 A 와 B의 유사도를 정량화 하는 데에 추가적으로 'Walking' 이라는 방식을 도입합니다. 해당 Walking 방식에 대해서는 Method Chapter에서 보완하겠습니다.

---

### Method

먼저 앞서 언급됐던 'Walking' 방식입니다. 간단히 설명하면, labeled data와 unlabeled data로부터 추출한 feature vector간 유사도 (M)을 이용하고, 이를 Softmax를 통해 class값을 유추해냅니다. 그 뒤 해당 feature로 부터 다시 유추하였을때, 원래의 label로 돌아올 수 있도록 최적화 하는 방식입니다. 수식화 하면 크게 3가지 확률의 값으로 나타내어 지는데, 아래와 같습니다.

1. Transition Probability
1. Round Trip Probability
1. Correct Walks Probability

위 세가지 Probability를 통하여 {{formula fontSize="SMALL"}}Loss_{walker}{{/formula}}를 정의합니다. {{formula fontSize="SMALL"}}Loss_{walker}{{/formula}} 에서는 visit한 뒤 돌아왔을 때, class값이 달라지면 penalty를 부여하여 optimization을 수행합니다. 이 때 Loss함수에 Uniform distribution을 적용하여, '동일한 class로만 돌아오면' 다른 이미지라 할지라도 penalty를 부여하지 않도록 하였습니다.

본 논문에서는 minimize할 대상의 Loss를 다음과 같이 정의합니다.

{{formula fontSize="SMALL"}}
Loss_{total} = Loss_{walker} + Loss_{visit} + Loss_{classification}
{{/formula}}

---

### Result

실험용 Dataset으로 연구팀은 MNIST와 STL-10, SVHN 데이터셋을 적용하였습니다. 그 결과로 Label이 매우 적을 때, 다른 state-of-art를 큰 차이로 능가하는 성능을 보였고, 현재 사용되는 Resnet과 같은 깊고 복잡한 망 구조를 이용한 것도 아닌데 높은 accuracy를 뽑아낼 수 있었습니다.
<img src="https://3.bp.blogspot.com/-rg_FZZ_vnK4/WVhuVNoYNCI/AAAAAAAAB5Q/jXcUP3kAPbscVhOBNX2u0XdHb-fnQhcBgCK4BGAYYCw/s1600/lba_8.png" width="500" height="300" />

---

(본 Note는 논문과 [참고Blog](http://jaejunyoo.blogspot.com/2017/07/learning-by-association-versatile-semi-supervised-training.html)를 참고하여 작성되었습니다.)
