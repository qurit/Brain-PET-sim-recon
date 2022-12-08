# Brain PET sim + recon
A C+Matlab-based PET simulation and reconstruction software, enabling realistic brain simulations, e.g. dynamic PiB PET imaging on the High Resolution Research Tomograph (HRRT).

*Arman Rahmim, PhD; Nov 1, 2019*

## Summary
- Analytic simulation of (dynamic) PET sinograms
- Begins with realistic computational human brain phantom
- Enables realistic, noisy simulations
- Statistical PET image reconstruction using 3D ordered-subset expectation maximization (OS-EM) algorithm
- Incorporate attenuation and normalization modeling
- Can simulate dynamic PET studies, with an initial table of kinetic K values; e.g. our default example performs dynamic PiB PET studies

## Reference
Please use the following reference if you publish results with help from this software tool:

A. Rahmim, Y. Zhou, J. Tang, L. Lu, V. Sossi, and D. F. Wong

Direct 4D parametric imaging for linearized models of reversibly binding PET tracers using generalized AB-EM reconstruction

Phys. Med. Biol., vol. 57, pp. 733-755, 2012.

## Folder Structure
There are two folders:
- `hrrt_open_2010-10-15_r234B`
- `HRRT_PiB_dynamic_brain_simulations`

The first one contains HRRT recon code, that you can re-compile. Makefile is in there.
This code is also used for analytic simulation, with realistic noise that can also be added to it.

Next, please go to the second folder and start with the file `HRRT_simulate_and_recon.cmd` It will need large amount of space available for a single noise realization.
For more noise realizations, to save space, it will overwrite existing sinograms with new noise realizations, but will still create NEW/non-overwritten reconstructed images.

## Configuration
The program has seven steps, which you can turn on or off
```
create_brain=1;
project_atten=1;
create_emission_images=1;
project_emission=1;
incorporate_atten_norm_and_noise=1;  
perform_reconstruction=1;
perform_parametric_estimation=1;
```
You probably don’t need the last one since it uses its own way of kinetic modeling to fit. You can do your own kind of kinetic modelling if you need to.

Kinetic parameters used to generate TACs for different regions are defined here in `generate_images_compartmental_modeling_256.m`

This program uses Eq. 16 and 17 in the paper of ours below which are standard compartmental modeling equations (2-tissue model).

> [3.5D dynamic PET image reconstruction incorporating kinetics-based clusters](https://rahmimlab.files.wordpress.com/2015/06/lu_pmb12_35d_dynamic_pet_image_reconstruction.pdf)
> -- <cite>Lijun Lu et al, Phys. Med. Biol., 2012.</cite>


The `dncat/` folder contains the code to create the brain regions, which are then fed to above `.m` matlab code to creates TACs.

**NOTE:** This uses our anthropomorphic brain phantom (also shared on our software page, but also included here).

The particular kinetic parameter numbers used in this code are for a PiB PET study. For instance as you’ll see in there, for PiB, the k1, k2, k3, k4 and vB values are:
```Matlab
% For PiB (cingulate was based on Anterior Cingulate measurements, Brain
% Stem based on Pons, and white matter based on subcortical white matter)       
% PiB -From the file: PiB ROI Matching - Julie.xlsx
values=[0.237 0.144	0.021	0.013  %Cingulate
        0.310	0.172	0.015	0.011  %Occipital cortex (Cx)
        0.278	0.147	0.018	0.016  %Anteroventral Striatum
        0.252	0.147	0.020	0.013  %Brain Stem (including Pons)
        0.288	0.151	0.010	0.012  %Cerebellum
        0.235	0.114	0.040	0.030  %Parietal Cx
        0.244	0.151	0.023	0.015  %Frontal Cx
        0.282	0.163	0.020	0.013  %Precuneus        
        0.232	0.130	0.017	0.014  %Lateral Temporal Cx
        0.251	0.150	0.017	0.013  %Somatosensory Cx
        0.178	0.112	0.019	0.017  %Mesial Temporal Cx
        0.104	0.087	0.067	0.020  %White matter
        0.268	0.109	0.020	0.028  %Thalamus
        0.278	0.155	0.018	0.014  %Occipital Pole
        0.271	0.151	0.016	0.012];%Occipotemporal Cx (mean of lateral temporal and occipital)
 ```
 

