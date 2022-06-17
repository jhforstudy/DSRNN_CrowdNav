# A tutorial for DSRNN-Crowd Navigation



This is a tutorial for installing DSRNN-Crowd Navigation.

[For a detailed explanation](https://github.com/Shuijing725/CrowdNav_DSRNN)



## 1. How to install

1. **Install Python 3.6** 

   The code may work with other versions of Python, but **Python 3.6** is highly recommended.

   Make sure that you need to use Python 3.6 as default, so you might have to use [Virtualenv](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/) or [Anaconda](https://www.cloudsigma.com/how-to-install-anaconda-on-ubuntu-18-04-in-six-simple-steps/).

2. **Clone the repository and install the required python package using pip.**

   ```
   pip install -r requirements.txt
   ```

3. **Install [OpenAI Baselines](https://github.com/openai/baselines#installation).**

   ```
   git clone https://github.com/openai/baselines.git
   cd baselines
   pip install -e .
   ```

4. **Install [Python-RVO2](https://github.com/sybrenstuvel/Python-RVO2) library.**

   ```
   git clone https://github.com/sybrenstuvel/Python-RVO2
   cd Python-RVO2
   pip install -r requirements.txt
   python setup.py build
   python setup.py install
   ```

5. **Finally, you have to check if you can import installed modules.**<br>If there's no error messages, modules are installed correctly.

   ```
   python
   >> import tensorflow
   >> import pandas
   >> import baselines
   >> import rvo2
   ```



If all modules are imported correctly, the next step is training & testing DS-RNN.



## 2. How to use

### Getting started
This repository is organized in three parts:

* ``crowd_sim/`` folder contains the simulation environment.
* ``crowd_nav/`` folder contains configurations and non-neural network policies.
* ``pytorchBaselines/`` contains the code for the DSRNN network and ppo algorithm.
<br>Below are the instructions for training and testing policies.

### Change configurations

* Environment configurations and training hyperparameters: ``modify crowd_nav/configs/config.py``<br>
* For FoV environment (left in the figure below): change the value of ``robot.FOV`` in ``config.py``
* For Group environment (right in the figure below): set ``sim.group_human`` to ``True`` in ``config.py``


### Run the code

1. **Train a policy.**

   ```
   python train.py
   ```

2. **Test policies.**

   Please modify the test arguments in the beginning of `test.py`.

   Two trained example weights for each type of robot kinematics are provided:

   - Holonomic: `data/example_model/checkpoints/27776.pt`
   - Unicycle: `data/example_model_unicycle/checkpoints/55554.pt`<br>

   ```
   python test.py
   ```

3. **Plot training curve.**

   ```
   python plot.py
   ```



## 3. Results

### Execute .py files

1. Trained about 10,000,000 timesteps for holonomic case.

```
Updates 0, num timesteps 360, FPS 637 
 Last 2 training episodes: mean/median reward -16.7/-16.7, min/max reward -19.0/-14.5

Updates 20, num timesteps 7560, FPS 645 
 Last 83 training episodes: mean/median reward -13.4/-15.9, min/max reward -36.1/15.4
 
 ...
```



2. Testing policies

```
2022-06-14 10:11:01, INFO: TEST  has success rate: 0.90, collision rate: 0.10, timeout rate: 0.00, nav time: 10.06, total reward: 20.8718
2022-06-14 10:11:01, INFO: Frequency of being in danger: 0.02 and average min separate distance in danger: 0.15
2022-06-14 10:11:01, INFO: TEST  has average path length: 190.30, CHC: 11.43
2022-06-14 10:11:01, INFO: Collision cases: 11 15 19 26 36 51 72 80 93 108 118 123 132 137 154 159 162 173 181 187 193 198 223 225 231 257 267 285 286 287 289 297 300 319 322 323 355 357 359 361 368 375 390 404 424 442 445 460 464 468
2022-06-14 10:11:01, INFO: Timeout cases: 9
 Evaluation using 500 episodes: mean reward 22.35157
```



3. Plotting training curve


![eprewmean](https://user-images.githubusercontent.com/48710703/173532606-7a8a5763-b33c-4367-adbc-bb6b2166144d.png)<br>

![value_loss](https://user-images.githubusercontent.com/48710703/173532615-c4094a23-ac50-4a1a-b3ee-6d2ea6694bb5.png)


### Code Review

#### Before training

In ``crowd_nav/configs/config.py``, you can change parameters for training.
* ``environment settings``
* ``human settings`` : size of human, maximum velocity, FoV, policy to control human
* ``robot settings`` : size of robot, maximum veloity, FoV
* ``reward function`` : rewards for your model
* ``training config`` : path for saving new model file or loading created model file, number of timesteps

#### After training

In ``data/YOUR_DATA/progress.csv`` and ``data/YOUR_DATA/checkpoints/YOUR_MODEL.pt``, you can find trained data.
* ``eprewmean`` : mean of reward for 1 update
* ``policy_entropy`` : randomness of actions an agent can take
* ``policy_loss`` : The mean magnitude of policy loss function. Correlates to how much the policy (process for deciding actions) is changing. The magnitude of this should decrease during a successful training session. These values will oscillate during training. Generally they should be less than 1.0.
* ``value_loss`` : The mean loss of the value function update. Correlates to how well the model is able to predict the value of each state. This should increase while the agent is learning, and then decrease once the reward stabilizes. These values will increase as the reward increases, and then should decrease once reward becomes stable.

