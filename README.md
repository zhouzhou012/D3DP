This experiment compared data augmentation schemes in D3DP, including diffusion-flipping, flipping once, and no flipping.

## Dependencies
Refer to the requirement file.



## Datasets

This experiment is evaluated on [Human3.6M](http://vision.imar.ro/human3.6m) 

### Human3.6M

We set up the Human3.6M dataset in the same way as [VideoPose3D](https://github.com/facebookresearch/VideoPose3D/blob/master/DATASETS.md).  You can download the processed data from [here](https://drive.google.com/file/d/1FMgAf_I04GlweHMfgUKzB0CMwglxuwPe/view?usp=sharing).  `data_2d_h36m_gt.npz` is the ground truth of 2D keypoints. `data_2d_h36m_cpn_ft_h36m_dbb.npz` is the 2D keypoints obatined by [CPN](https://github.com/GengDavid/pytorch-cpn).  `data_3d_h36m.npz` is the ground truth of 3D human joints. Put them in the `./data` directory.



## Evaluating models
You can download our pre-trained models, which are evaluated on Human3.6M (from [here](https://drive.google.com/file/d/1c48a2SxIkRxxqP5F1l3kpUSRWwHknYVK/view?usp=sharing)) and MPI-INF-3DHP (from [here](https://drive.google.com/file/d/1x78KEmAXJINPzJJbp7KP9RRjVZI5k-mt/view?usp=sharing)). Put them in the `./checkpoint` directory. 

### Human3.6M

To evaluate D3DP with JPMA using the 2D keypoints obtained by CPN as inputs, please run:
```bash
python main.py -k cpn_ft_h36m_dbb -c checkpoint -gpu 0 --nolog --evaluate h36m_best_epoch.bin -num_proposals 5 -sampling_timesteps 5 -b 4
```

You can balance efficiency and accuracy by adjusting `-num_proposals` (number of hypotheses) and `-sampling_timesteps` (number of iterations).

For visualization, please run:
```bash
python main_draw.py -k cpn_ft_h36m_dbb -b 2 -c checkpoint -gpu 0 --nolog --evaluate h36m_best_epoch.bin -num_proposals 5 -sampling_timesteps 5 --render --viz-subject S11 --viz-action SittingDown --viz-camera 1
```
The results will be saved in `./plot/h36m`.


## Training from scratch
### Human3.6M
To train model using the 2D keypoints obtained by CPN as inputs, please run:
```bash
python main.py -k cpn_ft_h36m_dbb -c checkpoint/model_h36m -gpu 0 --nolog
```
