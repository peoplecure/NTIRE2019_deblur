# A Deep Motion Deblurring Network based on Per-Pixel Adaptive Kernels with Residual Down-Up and Up-Down Modules
A source code of the 3rd winner of NTIRE 2019 Video Deblurring Challenge (*CVPRW*, 2019) : 
"A Deep Motion Deblurring Network based on Per-Pixel Adaptive Kernels with Residual Down-Up and Up-Down Modules" by Hyeonjun Sim and Munchurl Kim. [[pdf](http://openaccess.thecvf.com/content_CVPRW_2019/papers/NTIRE/Sim_A_Deep_Motion_Deblurring_Network_Based_on_Per-Pixel_Adaptive_Kernels_CVPRW_2019_paper.pdf)], [[NTIRE2019](http://www.vision.ee.ethz.ch/ntire19/)]

## Prerequisites
* python 2.7
* tensorflow (gpu version) >= 1.6 (The runtime in the paper was recorded on tf 1.6. But the code in this repo also runs in tf 1.13 )

## Testing with pretrained model
We provide the two test models depending on the training datasets, REDS (NTIRE2019 Video Deblurring Challenge:Track 1 Clean dataset [[pdf](http://openaccess.thecvf.com/content_CVPRW_2019/papers/NTIRE/Nah_NTIRE_2019_Challenge_on_Video_Deblurring_and_Super-Resolution_Dataset_and_CVPRW_2019_paper.pdf)], [[page](https://seungjunnah.github.io/Datasets/reds)]) and *GOPRO* ([[pdf](http://openaccess.thecvf.com/content_cvpr_2017/papers/Nah_Deep_Multi-Scale_Convolutional_CVPR_2017_paper.pdf)], [[page](https://github.com/SeungjunNah/DeepDeblur_release)]) with checkpoint in `/checkpoints_NTIRE/`, `/checkpoints_GOPRO/`, respectively. Download link to [/checkpoints_NTIRE/](https://www.dropbox.com/s/5uguucc85d0dn1d/checkpoints_NTIRE.zip?dl=0) and [/checkpoints_GOPRO/](https://www.dropbox.com/s/v6rj0cz3ix8vcx7/checkpoints_GOPRO.zip?dl=0)\
For NTIRE REDS dataset, our model was trained on the 'blur' and 'sharp' pair.\
For GOPRO dataset, our model was trained on the lineared blur (not gamma corrected) and sharp pair (as other state-of-the-art methods did).\
For example, to run the test model pretrained on GOPRO dataset, 
```bash
python main.py --pretrained_dataset 'GOPRO' --test_dataset './Dataset/YOUR_TEST/' --working_directory './data/'
```
or pretrained on NTIRE dataset with additional geometric self-ensemble (takes much more time), 
```bash
python main.py --pretrained_dataset 'NTIRE' --test_dataset './Dataset/YOUR_TEST/' --working_directory './data/' --ensemble
```

`test_dataset` is the location of the test input blur frames that should follow the format:
```
├──── Dataset/
   ├──── YOUR_TEST/
      ├──── blur/
        ├──── Video0/
           ├──── 0000.png
           ├──── 0001.png
           └──── ...
        ├──── Video1/
           ├──── 0000.png
           ├──── 0001.png
           └──── ...
        ├──── ...
```
The deblurred output frames will be generated in `working_directory` as,
```
├──── data/
   ├──── test/
     ├──── Video0/
        ├──── 0000.png
        ├──── 0001.png
        └──── ...
     ├──── Video1/
        ├──── 0000.png
        ├──── 0001.png
        └──── ...
     ├──── ...
```

### Evaluation
To calcuate PSNR between the deblurred output and the corresponding sharp frames,
```bash
python main.py --phase 'psnr'
```
Before that, the corresponding sharp frames should follow the format:,
```
├──── Dataset/
   ├──── YOUR_TEST/
      ├──── sharp/
        ├──── Video0/
           ├──── 0000.png
           ├──── 0001.png
           └──── ...
        ├──── Video1/
           ├──── 0000.png
           ├──── 0001.png
           └──── ...
        ├──── ...
```
Our NTIRE model had yielded average 33.86 and 33.38 dB PSNR for 300 validation frames with and without self-ensemble, respectively.\
For GOPRO benchmark test dataset,
| Method | PSNR(dB) | SSIM |
|---|---|---|
|Nah et al. | 28.62 | 0.9094|
|Tao et al. | 30.26 | 0.9342|
|---|---|---|
|Ours | 31.34 | 0.9474|


## Reference
```bibtex
@inproceedings{sim2019deep,
  title={A Deep Motion Deblurring Network Based on Per-Pixel Adaptive Kernels With Residual Down-Up and Up-Down Modules},
  author={Sim, Hyeonjun and Kim, Munchurl},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition Workshops},
  year={2019}
}
```
## Contact
Please send me an email, flhy5836@kaist.ac.kr
