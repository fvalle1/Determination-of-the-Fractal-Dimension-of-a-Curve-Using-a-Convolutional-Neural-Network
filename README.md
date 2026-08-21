# Master's Thesis: Determination of the Fractal Dimension of Curves Using a Convolutional Neural Network

## Reproducibility

[![RepoReady](https://img.shields.io/endpoint?url=https%3A%2F%2Fapi.repoready.ai%2Fapi%2Fbadges%2Fc3068d3c-ac38-4460-9be5-3ee5c5149144%2Fbadge)](https://app.repoready.ai/shared/badges/c3068d3c-ac38-4460-9be5-3ee5c5149144)

This repository has been analyzed by [RepoReady](https://repoready.ai) for reproducibility. The score indicates how well the codebase follows best practices for documentation, code quality, and reproducibility standards. Click the badge to view the full report.

**University of Turin**

**Supervisor**: Prof. Guido Boffetta

**Co-Supervisor**: Prof. Matteo Osella

**External Examiner**: Dr. Filippo Valle


## Abstract

In this project I created a CNN (Convolutional Neural Network) for the determination of the fractal dimension of curves.
The main aspect of this work is the use of the Schramm-Loewner Evolution to create a dataset of curves for the training of the CNN. 
Thanks to the property of this equation I could associate the theoretical fractal dimension with each curve generated. 
Similar works only uses the box counting fractal dimension.
This allowed me to compare the performance of the CNN with the one of the box counting technique.
The findings show an overall better result for the CNN.
I finally used the CNN and box counting to determine the fractal dimensions of portions of the West Coast of Great Britain.


## Folders

Inside **Model** you will find the script CNN.ipynb for training a CNN.
In order to train mine I used the GPU available inside google Colab.
The two CNN I trained are under MainModel and No_Aug Model, with their respective training history.

Inside **CNN Estimate** you will find the script to evaluate the performance of the CNN in the three conditions studied: 
- curves with gaps
- curves with noise
- curves with limited number of points (portion)

The equivalent scripts for the box counting can be found inside the folder **Box Counting Estimates**.

The script for the creation of the curves can be found inside the folder **Dataset Creation**.

**Numerical Results** section contains the results, saved as .CSV files for both the CNN and the Box Counting in the three conditions studied.

**Results** holds the script used for comparing the performances.

Finally, inside **Coast Analysis** can be found the script used for the estimation of the fractal dimension of the West Coast of Great Britain.
Note that you need to downaload the .shp file of the UK from GADM (https://gadm.org/).

## Overview

I used the algorithm described in the article "A Fast Algorithm for Simulating the Chordal Schramm–Loewner Evolution", by Tom Kennedy, to create fractal curves. 
The Schramm-Loewner Evolution equation is:

$$
\frac{\partial g_t(z)}{\partial t}
= \frac{2}{g_t(z) - \sqrt{\kappa}\ B_t},
\qquad g_0(z) = z
$$

The parameter **K** is linked with the fractal dimension **D**, of the curve generated, by the equation:

$$
D = 1 + \frac{k}{8}
$$

I used this property to create a dataset of curves with labels given by the theoretical fractal dimension.
The curves are made of 20.000 points, and I store them as NumPy arrays.
The dataset is then used to train a CNN, with the aim of creating a model capable of outperforming the Box Counting technique.

The Box Counting suffers of some limitations:
- it can not work with discontinuos curves
- the image must contain only the curve
- with a limited portion of the curve it can underestimate the fractal dimension

These limitations are the reasons why I introduce three forms of data augmentations:
- curves with gaps (from 0 to 9 gaps)
- images of curves with noise (from 0 to 7.5%)
- images of portion of curves (from 20.000 to 1.000 points)

In order to evaluate the impact of this specific form of training I then trained a second CNN without any form of data augmentation.

The results shows that the CNN performs significantly better than the Box Counting in all three cases, with remarkable results also outside the parameters of training (red background). 

Portions of curves:
<img width="1361" height="707" alt="portions" src="https://github.com/user-attachments/assets/a01bd5c9-2308-474d-a795-64ba4e7271a5" />
Images with noise:
<img width="1331" height="699" alt="noise" src="https://github.com/user-attachments/assets/0af2fea1-3f64-4d4a-8740-ca8ed33b9db6" />
Curves with gaps:
<img width="1362" height="714" alt="gaps" src="https://github.com/user-attachments/assets/52016415-e8ce-4dfd-a85f-0f8dae1d07d9" />

I finally used the CNN to study the West Coast of Great Britain.
I divided it in two different ways: 73 portions of 10.000 point, and 13 of 56.153 points.
In this way I could verify how the performance changes at different scales.

I represent the results with a CNN vs Box Counting plot and with a color scale of the different portion of coastline.

For the 73 portions:

<img width="745" height="483" alt="73" src="https://github.com/user-attachments/assets/7c7358e4-527d-469f-85f4-221f14b8fca8" />

<img width="589" height="547" alt="plot73" src="https://github.com/user-attachments/assets/ec7f2145-2aea-46a3-be5d-f7b5ca5176c5" />

For the 13 portions:

<img width="772" height="493" alt="13" src="https://github.com/user-attachments/assets/62825d09-45d8-45d5-b2b8-faa40e3ed470" />

<img width="589" height="547" alt="plot13" src="https://github.com/user-attachments/assets/8ac20b17-5622-4fc3-983f-9e38e2c67c79" />

The CNN and the box-counting method provide qualitatively consistent results.
The Box Counting has the tendency to underestimate the fractal dimension, as was also visible in the synthetic curves. This led me to believe that the CNN provides the most reliable results.
