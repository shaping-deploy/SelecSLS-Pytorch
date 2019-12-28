# SelecSLS Convolutional Net Pytorch Implementation
Reference ImageNet implementation of SelecSLS Convolutional Neural Network architecture proposed in [XNect: Real-time Multi-person 3D Human Pose Estimation with a Single RGB Camera](https://arxiv.org/abs/1907.00837).

The network architecture is 1.3-1.5x faster than ResNet-50, particularly for larger image sizes, with the same level of accuracy on different tasks! 
Further, it takes substantially less memory while training, so it can be trained with larger batch sizes!

### Update (28 Dec 2019)
Better and more accurate models / snapshots are now available!

## ImageNet results
    
<table>
  <tr>
    <th></th>
    <th colspan="6">Forward Pass Time (ms)<br>for different image resolutions</th>
    <th colspan="2">ImageNet<br>Accuracy</th>
  </tr>
  <tr>
    <td></td>
    <td colspan="2">512x512</td>
    <td colspan="2">400x400</td>
    <td colspan="2">224x224</td>
    <td>Top-1</td>
    <td>Top-5</td>
  </tr>
  <tr>
    <td>Batch Size</td>
    <td>1</td>
    <td>16</td>
    <td>1</td>
    <td>16</td>
    <td>1</td>
    <td>16</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>ResNet-50</td>
    <td>15.0</td>
    <td>175.0</td>
    <td>11.0</td>
    <td>114.0</td>
    <td>7.2</td>
    <td>39.0</td>
    <td>23.9</td>
    <td>7.1</td>
  </tr>
  <tr>
    <td>VoVNet-39</td>
    <td>13.0</td>
    <td>197.0</td>
    <td>10.8</td>
    <td>130.0</td>
    <td>6</td>
    <td>41.0</td>
    <td>23.2</td>
    <td>6.6</td>
  </tr>
  <tr>
    <td>SelecSLS-60</td>
    <td>11.0</td>
    <td>115.0</td>
    <td>9.5</td>
    <td>85.0</td>
    <td>7.3</td>
    <td>29.0</td>
    <td>23.8</td>
    <td>7.0</td>
  </tr>
  <tr>
  </tr>
  <tr>
    <td>SelecSLS-42_B</td>
    <td>11.0</td>
    <td>115.0</td>
    <td>9.5</td>
    <td>85.0</td>
    <td>7.3</td>
    <td>29.0</td>
    <td>22.9</td>
    <td>6.6</td>
  </tr>
  <tr>
    <td>SelecSLS-60</td>
    <td>11.0</td>
    <td>115.0</td>
    <td>9.5</td>
    <td>85.0</td>
    <td>7.3</td>
    <td>29.0</td>
    <td>22.1</td>
    <td>6.1</td>
  </tr>
  <tr>
    <td>SelecSLS-60_B</td>
    <td>11.0</td>
    <td>115.0</td>
    <td>9.5</td>
    <td>85.0</td>
    <td>7.3</td>
    <td>29.0</td>
    <td>21.6</td>
    <td>5.8</td>
  </tr>
</table>

The inference time for all models above is measured on a TITAN X GPU using the accompanying scripts. The accuracy results for ResNet-50 are from torchvision, and the accuracy results for VoVNet-39 are from [VoVNet](https://github.com/stigma0617/VoVNet.pytorch).

# SelecSLS (Selective Short and Long Range Skip Connections)
The key feature of the proposed architecture is that unlike the full dense connectivity in DenseNets, SelecSLS uses a much sparser skip connectivity pattern that uses both long and short-range concatenative-skip connections. Additionally, the network architecture is more amenable to filter/channel pruning than ResNets.
You can find more details in the [paper](https://arxiv.org/abs/1907.00837).

Another recent paper proposed the VoVNet architecture, which shares some design similarities with our architecture. However, as shown in the above table, our architecture is significantly faster than both VoVNet-39 and ResNet-50 for larger batch sizes as well as larger image sizes.

## Usage
This repo provides the model definition in Pytorch, trained weights for ImageNet, and code for evaluating the forward pass time
and the accuracy of the trained model on ImageNet validation set. 
In the paper, the model has been used for the task of human pose estimation, and can also be applied to a myriad of other problems as a drop in replacement for ResNet-50.

```
wget http://gvv.mpi-inf.mpg.de/projects/XNectDemoV2/content/SelecSLS60_statedict.pth -o ./weights/SelecSLS60_statedict.pth
python evaluate_timing.py --num_iter 100 --model_class selecsls --model_config SelecSLS60 --input_size 512 --gpu_id <id>
python evaluate_imagenet.py --model_class selecsls --model_config SelecSLS60 --model_weights ./weights/SelecSLS60_statedict.pth --gpu_id <id> --imagenet_base_path <path_to_imagenet_dataset>
```

## Pretrained Models
- [SelecSLS-60](http://gvv.mpi-inf.mpg.de/projects/XNectDemoV2/content/SelecSLS60_statedict.pth)

## Requirements
 - Python 3.5
 - Pytorch >= 1.1

## Citing
If you find use for the model in your work, please cite:

```
@article{mehta2019xnect,
  title={XNect: Real-time Multi-person 3D Human Pose Estimation with a Single RGB Camera},
  author={Mehta, Dushyant and Sotnychenko, Oleksandr and Mueller, Franziska and Xu, Weipeng and Elgharib, Mohamed and Fua, Pascal and Seidel, Hans-Peter and Rhodin, Helge and Pons-Moll, Gerard and Theobalt, Christian},
  journal={arXiv preprint arXiv:1907.00837},
  year={2019}
}
```



