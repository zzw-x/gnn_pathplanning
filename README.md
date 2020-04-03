# PyTorch Project for Graph Neural Network based MAPF
Code accompanying the paper

[Graph Neural Networks for Decentralized Multi-Robot Path Planning](https://arxiv.org/abs/1912.06095) 

from Qingbiao Li (1), Fernando Gama (2), Alejandro Ribeiro (2), Amanda Prorok (1) at University of Cambridge (1) and at University of Pennsylvania (2).

### Table of Contents: 
<!-- Table of contents generated generated by http://tableofcontent.eu -->
- [Graph MAPF project](#pytorch-project-template)
    - [Project Diagram](#template-class-diagram)
    - [Framework Structure](#repo-structure)
    - [How to use this repo](#use-repos)
    - [Requirements](#requirements)
    - [License](#license)
    - [Citation](#Citation)
    

### Project Diagram:
![alt text](utils/assets/class_diagram.png "Template Class diagram")

### Framework Structure:
The repo has the following structure:
```
├── agents (overall framework for training and testing)
|  └── base.py
|  └── decentralplannerlocal.py
|  └──      (DCP) 
|  └── decentralplannerlocal_OnlineExpert.py
|  └──      (DCP with onlin expert mechanism) 
|
├── configs (set up key parameters for training and inference stage,)
|  └── dcp_ECBS.json
|  └── dcp_onlineExpert.json
|
├── dataloader (load data for training)
|  └── Dataloader_dcplocal_notTF_onlineExpert.py
|
├── graphs 
|  └── models (model including CNN -> GNN -> MLP)
|  |  |
|  |  └── decentralplanner.py 
|  |
|  └── losses
|     └── cross_entropy.py
|
├── utils
|  |
|  └── assets
|  |  └── dataTools.py
|  |  └── graphML.py
|  |  └── graphTools.py
|  |
|  └── multirobotsim_dcenlocal.py 
|  └──      (simulator for dencentral agents)
|  └── multirobotsim_dcenlocal_onlineExpert.py 
|  └──      (simulator for dencentral agents with online expert mechanism, where failure is saved.)
|  └── visualize.py 
|  └──      (visualize the predicted path with communcation link.)
|  └── visualize_expertAlg.py
|  └──      (visualize the ground truth path.)
|  └── metrics.py 
|  └──      (Record stastics during inference stage.)
|  └── config.py 
|
├── offlineExpert
|  |
|  └── CasesSolver.py
|  └──       1, (# generate map) Randomly generate map with customized obstacle density and obstacle, 
|  └──       2. (# case under a map)
|  └──           At each specific map, generate random pairs of start and goal position for each agents.
|  └──       3. (for given case) Apply expert algorithm to compute solution.
|  |
|  └── DataGen_Transformer.py
|  └──      (Transform the solution into specific data format that ready to be loaded by dataloader.
|  └──          including: map, input tensor wiith each agents paths, GSO.)
|
├── onlineExpert
|  |
|  └── ECBS_onlineExpert.py
|  └──      (Apply expert algorithm to compute solution for failture cases recorded during training process.)
|  |
|  └── DataTransformer_local_onlineExpert.py
|  └──      (Transform the solution into specific data format, and then merged into offline dataset.)
|
├── experiments
|
├── data
|
├── statistic_analysis 
|  └── (Fig.3.) result_analysis_errorbar.py 
|  └── (Fig.4.) result_analysis_generalization_colormap.py
|  └── (Fig.5.) result_analysis_hist_impact_3K.py
|
└──  main.py

```


### Requirements:
```
easydict>=1.7
matplotlib>=3.1.2
numpy>=1.14.5
Pillow>=5.2.0
scikit-image>=0.14.0
scikit-learn>=0.19.1
scipy>=1.1.0
tensorboardX>=1.2
torch>=1.1.0
torchvision>=0.3.0
```
### How to use this repo:

#### Test trained network, for exmaple DCP OE - K=3
1. [Download](https://drive.google.com/drive/folders/1Cq6-U4n0dhrXC_yJGo8JgnZWqsA5Pz5g?usp=sharing) the dataset and trained network.
2. changes the 'data_root' and 'save_data' in ./configs/dcp_onlineExpert.json and then run
```
python main.py configs/dcp_onlineExpert.json --mode test --log_anime  --best_epoch --test_general --log_time_trained 1582034757   --nGraphFilterTaps 3 --map_w 20  --num_agents 10  --trained_num_agents 10 --trained_map_w 20
```
#### Train a new network, with DCP OE - K=3
```
python main.py configs/dcp_onlineExpert.json --mode train  --map_w 20 --nGraphFilterTaps 3  --num_agents 10  --trained_num_agents 10
```

More setting can be found in scrips

#### Visualization
```
python ./utils/visualize.py --map [Path_to_Cases]/successCases_ID00000.yaml --schedule  [Path_to_Cases]/predict_success/successCases_ID00000.yaml --GSO  [Path_to_Cases]/GSO/successCases_ID00000.mat --speed 2 --video [predict_success]/video.mp4 --nGraphFilterTaps 2 --id_chosenAgent 0
```
where [Path_to_Cases] is defined by where the 'Results/AnimeDemo'.

### License:
This work based on a **Scalable template**  by [Hager Rady](https://github.com/hagerrady13/) and [Mo'men AbdelRazek](https://github.com/moemen95)

The graph neural network module of this work based on the [GNN library](https://github.com/alelab-upenn/graph-neural-network) from Alelab at University of Pennsylvania.

The project of graph mapf is licensed under MIT License - see the LICENSE file for details

### Citation:
If you use this paper in an academic work, please cite:
```
@article{li2019graph,
  title={Graph Neural Networks for Decentralized Multi-Robot Path Planning},
  author={Li, Qingbiao and Gama, Fernando and Ribeiro, Alejandro and Prorok, Amanda},
  journal={arXiv preprint arXiv:1912.06095},
  year={2019}
}
```
