## Peephole: Predicting Network Performance Before Training
#### Deng, B., Yan, J., & Lin, D. (2017). arXiv preprint arXiv:1712.03351.

Improving network design has been hard work.
Good network should be used to obtain good performance for a given data. But there is not a clear answer, what good network is.
One of the good network is deeper network. But, Large design space spends tremendous cost required for network training poses.
Another one is decide many things to design good network. For example, in CNN, what kind of layers to place in what order, how many kernel sizes and filters to each layer, how to determine learning rates, etc.
So It is hard to get a clear guide to network design.

##### In this work, they propose a new approach to this problem, predicting the performance of a network before training, based on its architecture.


![fig1](https://github.com/Oh-Yoojin/Peephole/blob/master/image/fig1.png)

Traditional network design measures accuracy by learning from train dataset about designs created by human beings.
    - It takes long time
In new process, if network designer suggest network architecture, It predicts accruacy in just one second by using Peephole and then suggests new network.

##### Basic idea of the Peephole
![idea](https://github.com/Oh-Yoojin/Peephole/blob/master/image/idea.png)

Puts the Long Short-Term Memory (LSTM) forms in the network structure and receives the performance of that network from the output.
* The biggest difference from previous studies is it predicts network performance by only the network architecture itself.

##### How to convert network structure to input forms
##### Unified Layer Code
![convert](https://github.com/Oh-Yoojin/Peephole/blob/master/image/convert.png)

Convert network structure to input forms: Unified Layer Code (ULC design)
ULC assigns one label for each type of layer and add kernel size and channel number contain information about layer.
* TY is type id
* KW is kernel width
* KH is kernel height
* CH is channel number

Expresses channel number as a percentage of input/output rate.
```
input layer : 64,
output channels : 16
-> CH = 0.25
```

* layer embedding : (TY, KW, KH, CH)

##### Peephole structure
![structure](https://github.com/Oh-Yoojin/Peephole/blob/master/image/structure.png)

The values for the network architecture are subsequently entered into LSTM and the last output determined by the value of LSTM hidden state.
This feature value is combined with the learning epoch vector. The learning epoch vector enters the Multi-layer perceptron (MLP).
Finally we can get the output.

#### Training Peephole
##### Block-based Generation design
Randomly layer combination is inefficient(like 3 consecutive activation layers)
![fig4](https://github.com/Oh-Yoojin/Peephole/blob/master/image/fig4.png)

* Skeleton + generated blocks
  - One block contains less than 10 layers
  First layer is convolution layer. kernel size is random.
  - Use Markov chain for resonable layer combinations
  ![markov](https://github.com/Oh-Yoojin/Peephole/blob/master/image/markov.PNG)
  
  - Number of convolution layers within a block is less than 4.
  - 1×1 convolution for dimension matching (for resolution problem)

##### Learning Objective
![loss](https://github.com/Oh-Yoojin/Peephole/blob/master/image/loss.png)

#### Experiments
##### Datasets
![table2](https://github.com/Oh-Yoojin/Peephole/blob/master/image/table2.png)

##### Comparison of Prediction Accuracies
Methods to compare.
1) Bayesian Neural Network (BNN) : Use initial 24% part of learning curve to yield probabilistic extrapolations
2) v-Support Vector Regression (v-SVR) : Use initial part of learning curve and simple heuristic feature to predict performance of a network

![table34](https://github.com/Oh-Yoojin/Peephole/blob/master/image/table34.png)

![fig5](https://github.com/Oh-Yoojin/Peephole/blob/master/image/fig5.png)

The predictions made by Peephole demonstrate notably higher correlation with the actual values than those from other methods, especially at the high-accuracy area.

##### Transfer to ImageNet
Directly training Peephole based on ImageNet is prohibitively expensively due to the lengthy processes of training a network on ImageNet.

![table5](https://github.com/Oh-Yoojin/Peephole/blob/master/image/table5.png)

Select the network architecture with the highest Peephole predicted accuracy among those in validation set for CIFAR-10, then scale it up and transfer it to ImageNet.

#### Conclusion
There are no other parameters considered that affect the performance of the network.

Using same structure of the network skeleton in the test data is mispoint. Different forms of network skeleton need to be used to accurately measure reliability.
