## Social NCE: Contrastive Learning of Socially-aware Motion Representations

<p align="center">
  <img src="docs/illustration.png" width="300">
</p>

This is a PyTorch implementation of the [Social-NCE paper](https://arxiv.org/abs/2012.11717). 

```bibtex
@article{liu2020snce,
  title   = {Social NCE: Contrastive Learning of Socially-aware Motion Representations},
  author  = {Yuejiang Liu and Qi Yan and Alexandre Alahi},
  journal = {arXiv preprint arXiv:2012.11717},
  year    = {2020}
}
```

> Social Contrastive Learning + Knowledge-driven Negative Sampling &#129138; Robust Neural Motion Models
> * Rank the **1st place** on the [Trajnet++ forecasting challenge](https://www.aicrowd.com/challenges/trajnet-a-trajectory-forecasting-challenge/leaderboards) at the time of publication
> * SOTA on imitation / reinforcement learning for [autonomous navigation in crowds](https://github.com/vita-epfl/CrowdNav)

### Preparation

Setup environments follwoing the [SETUP.md](docs/SETUP.md)

### Training & Evaluation

* Behavioral Cloning (Vanilla)
  ```
  python imitate.py --contrast_weight=0.0 --gpu
  python test.py --policy='sail' --circle --model_file=data/output/imitate-baseline-data-0.50/policy_net.pth
  ```
* Social-NCE + Conventional Negative Sampling (Local)
  ```
  python imitate.py --contrast_weight=2.0 --contrast_sampling='local' --gpu
  python test.py --policy='sail' --circle --model_file=data/output/imitate-local-data-0.50-weight-2.0-horizon-4-temperature-0.20-nboundary-0-range-2.00/policy_net.pth
  ```
* Social-NCE + Safety-driven Negative Sampling (Ours)
  ```
  python imitate.py --contrast_weight=2.0 --contrast_sampling='event' --gpu
  python test.py --policy='sail' --circle --model_file=data/output/imitate-event-data-0.50-weight-2.0-horizon-4-temperature-0.20-nboundary-0/policy_net.pth
  ```
* Method Comparison
  ```
  bash script/run_vanilla.sh && bash script/run_local.sh && bash script/run_snce.sh
  python utils/compare.py
  ```

### Basic Results

Results of behavioral cloning with different methods.

<img src="docs/collision.png" height="240"/> <img src="docs/reward.png" height="240"/> 

Averaged results from the 150th to 200th epochs.

<table><tbody>
<!-- START TABLE -->
<!-- TABLE HEADER -->
<th valign="bottom"></th>
<th valign="bottom">collision</th>
<th valign="bottom">reward</th>
<!-- TABLE BODY -->
<tr><td align="left">Vanilla</td>
<td align="center">12.7% &#177; 3.8%</td>
<td align="center">0.274 &#177; 0.019</td>
<tr><td align="left">Local</td>
<td align="center">19.3% &#177; 4.2%</td>
<td align="center">0.240 &#177; 0.021</td>
<tr><td align="left">Ours</td>
<td align="center">2.0% &#177; 0.6%</td>
<td align="center">0.331 &#177; 0.003</td>
</tr>
</tbody></table>

### Acknowledgments

Our code is developed based on [CrowdNav](https://github.com/vita-epfl/CrowdNav). 
