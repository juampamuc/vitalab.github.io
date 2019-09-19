---
layout: review
title: "(FocalNet) Joint Prostate Cancer Detection and Gleason Score Prediction in mp-MRI via FocalNet"
tags: medical segmentation CNN
author: "Pierre-Marc Jodoin"
cite:
    authors: "R. Cao, A.M. Bajgiran, S.A. Mirak, S. Shakeri, X. Zhong, D. Enzmann, S. Raman, and K. Sung"
    title:   "Joint Prostate Cancer Detection and Gleason Score Prediction in mp-MRI via FocalNet"
    venue:   "IEEE TMI 2019"
pdf: ""
---


# Highlights

![](/article/images/MFL/sc00.png)

In this paper, the authors present a method called FocalNet to segment prostate lesions and predict its Gleason score (GS) based on T2 and ADC images.  Results obtained on a record of 417 patients reveal SOTA performances.  The main contribution of this paper is their "ordinal class-encoding" to characterize lesion aggressiveness and mutual finding loss to fully exploit knowledge in the multi-modality images.

# Method

Their ordinal encoding in shown in Table 1.  Instead of the one-hot-vector, they use  an encoding which accounts for the similarity between the agressiveness of the various lesions. 

![](/article/images/MFL/sc03.png)


Their overall method is shown in Figure 3.

![](/article/images/MFL/sc02.png)

At first they define the following Focal loss which is used instead of the CE loss to account for the class imbalance


![](/article/images/MFL/sc04.png)

They also train their system to be effective at detecting image features into only one modality (i.e. T2 or ADC) through a mutual finding loss (MFL)



![](/article/images/MFL/sc05.png)


Where L2 stands for the L2 distance between the prediction with both modality and the prediction of only one modality.

The overall loss is given as follows :


![](/article/images/MFL/sc06.png)



## Results

Results obtained on 400 patients are very close to that of an expert

![](/article/images/MFL/sc07.png)

![](/article/images/MFL/sc08.png)

