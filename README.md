# SAC-Discrete in PyTorch
This is a PyTorch implementation of SAC-Discrete([Christodoulou. 2019](https://arxiv.org/abs/1910.07207)) with **Prioritized experience replay**([Schaul et al. 2015](https://arxiv.org/abs/1511.05952)) and **Multi-step learning**, which is used in Rainbow([Hessel et al. 2017](https://arxiv.org/abs/1710.02298)).

I referred to some implementations below.
- [Authors's implementation](https://github.com/p-christ/Deep-Reinforcement-Learning-Algorithms-with-PyTorch) (however, without codes for atari's envs.)
- [The implementation of SAC](https://github.com/ku2482/soft-actor-critic.pytorch) (for continuous control.)

## Requirements
You can install liblaries using `pip install -r requirements.txt`.

## Usage

### NOTE

I changed some implementations to get more stable trainings or easy implementations.

- Prioritized experience replay
  - You can use PER with `--per`.
- Multi-step learning
  - You can use Multi-step learning with `--multi_step ${int number}` (default is 3).

- Target update
  - In original paper, target network is updated as **hard update** with fixed interval.
  - I updated target network as **soft update**, however, which doesn't seem to influence performance.
  - You can use **hard update** with `--update_type 'hard'` (default is 'soft').
- Gradient norm clipping
  - I use **gradient norm clipping** as author's implementation.
  - You can specify clipping range with `--grad_clip ${float number} (default is 5.0)`
- Entropy target annealing
  - In original paper, target entropy is fixed as `np.log(action_space.n)*0.98`, which is maximum entropy multiplied by 0.98.
  - I found this to large (not sure), and instead I annealled entropy target from `np.log(action_space.n)*0.98` to `np.log(action_space.n)*0.98*(1-target_annealing_ratio)` during training.
  - You can use original (fixed) target with `--target_annealing_ratio 0.0` (default is 0.0).



### Training

You can train SAC-Discrete in atari envs.

```
python core/main.py --cuda --env_name ${name of environment}
```

For example, the score after training 100,000 steps in **MsPacman** using command below is around 600, which is comparable with the paper.

```
python core/main.py --cuda --env_name MsPacmanNoFrameskip-v4 --target_annealing_ratio 0.4
```

### Result

<img src="https://user-images.githubusercontent.com/37267851/67809830-c9fc1200-fadc-11e9-8f48-799a19689dd6.gif" title="MsPacman" width=250>
