# A tutorial for DSRNN-Crowd Navigation



This is a tutorial for installing DSRNN-Crowd Navigation

[For a detailed explanation](https://github.com/Shuijing725/CrowdNav_DSRNN)



## 1. How to install

1. Install Python3.6 (The code may work with other versions of Python, but 3.6 is highly recommended).

   Make sure that you need to use Python 3.6 as default, so you might have to use [Virtualenv](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/).

   

2. Clone the repository and install the required python package using pip. For pip, use the following command:

   ```
   pip install -r requirements.txt
   ```

3. Install [OpenAI Baselines](https://github.com/openai/baselines#installation).

   ```
   git clone https://github.com/openai/baselines.git
   cd baselines
   pip install -e .
   ```

4. Install [Python-RVO2](https://github.com/sybrenstuvel/Python-RVO2) library.

   ```
   git clone https://github.com/openai/baselines.git
   cd baselines
   pip install -e .
   ```

5. Finally, you have to check if you can import installed modules.

   ```
   python
   >> import tensorflow
   >> import pandas
   >> import baselines
   ```



If all modules are imported correctly, the next step is training & testing DS-RNN.



## 2. How to use

1. Train a policy.

   ```
   python train.py
   ```

2. Test policies.

   Please modify the test arguments in the beginning of `test.py`.

   Two trained example weights for each type of robot kinematics are provided:

   - Holonomic: `data/example_model/checkpoints/27776.pt`
   - Unicycle: `data/example_model_unicycle/checkpoints/55554.pt`

   ```
   python test.py
   ```

3. Plot training curve.

   ```
   python plot.py
   ```
