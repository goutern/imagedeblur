# Deblured Image Quality Assesment
By: Nicholas Goutermout, Carley Reardon, Sidd Shanmugam

# Introduction
This is a project that compare four image debluring methods and measure how effective they are at debluring video frames. 

## Deblurring Scripts
After ensuring you have the correct python dependencies.
The scripts work from a folder "frames" in the working directory. 
If you want to just deblur frames you can place them in the frames directory. Otherwise if you wish to deblur frames from a video you will need to run the frameSeparator.ipynb on your video. 
If you wish to deblur using deep learning methods you will need to install the following resources
- GAN  https://github.com/VITA-Group/DeblurGANv2
- Gated Fusion Network (GFN) https://github.com/jacquelinelala/GFN

Our debluring scripts for machine learning (non-DL) methods can be found in the following files: 
- [wiener_deconv.ipynb](https://github.com/goutern/imagedeblur/blob/main/wiener_deconv.ipynb "wiener_deconv.ipynb")
- [out_of_focus.ipynb](https://github.com/goutern/imagedeblur/pull/4/files#diff-74339cc3dffc4cc4e2e1571d8e8fa75082890e420c6191b5a4307fcfc81bb703 "out_of_focus.ipynb")

For these methods, we used the Wiener Deconvolution method, which is a mathematical technique that aims to minimize noise in blurred images due to poor signal-to-noise ratio. You can read more about this method on this Wikipedia page: https://en.wikipedia.org/wiki/Wiener_deconvolution For the image deblurring application, we used two approaches:
- Wiener Deconvolution with motion-based point-spread function to represent the images' impulse response (used in [wiener_deconv.ipynb](https://github.com/goutern/imagedeblur/blob/main/wiener_deconv.ipynb "wiener_deconv.ipynb"))
- Wiener Deconvolution with out-of-focus point-spread function to represent the images' impulse response (used in [out_of_focus.ipynb](https://github.com/goutern/imagedeblur/pull/4/files#diff-74339cc3dffc4cc4e2e1571d8e8fa75082890e420c6191b5a4307fcfc81bb703 "out_of_focus.ipynb"))

In our specific problem, the motion-based application tended to work better due to our video being of objects moving on a conveyor belt. However, the success of these methods will depend on the problem.

In our runs we changed the machine learning scripts to output to custom fram directorys for our measument scripts to read. For example the for GAN we output to gan_frames. 

Once you have sperated the video frames and generated the required deblured frames we can use the measument scripts to compare the results. 


## Image Quality Evaluation Scripts

There are three measuments scripts though after expirimentation we found only two of these methods to be useful. The traditional PSNR and SSIM do not work for this case because we do not have unblurred baseline data to reference as a ground truth. 
We instead use two no-reference "blind" image measurment techiques BRISQUE and NIQE.

BRISQUE a.k.a the “Blind/Referenceless Image Spatial Quality Evaluator” works from the assumption that distorted images pixel intensities follow a Gaussian distribution. This metrics normalizes the image using Mean Subtracted Contrast Normalization (MSCN), and creates subimages to compare using Generalized Gaussian Distribution (GGD) and Asymmetric Generalized Gaussian Distribution (AGGD) to create feature vectors. Then an SVM is used to predict the quality score of the image. Further explanation on BRISQUE can be found in [1]. 

To measure using BRISQUE adjust [brisque.ipynb](https://github.com/goutern/imagedeblur/blob/main/brisque.ipynb "brisque.ipynb") to reference your deblured images. Please note if you have a lot of images this may take a long time and use a lot of memory.

NIQE a.k.a the “Natural Image Quality Evaluator” is an IQA that is no-reference, opinion-unaware, and distortion-unaware. This metric determines the quality of an image by measuring the distance between a multivariate gaussian (MVG) model based on the natural scene statistic (NSS) features of the image, to the MVG based on “quality aware” features that were previously extracted from a corpus of natural images. NIQE strives to be distortion-unaware by extracting features only from natural images (not intentionally distorted ones), so the model is not limited to a specific set of distortion types. Further explanation on NIQE can be found in [2]. 

To measure using NIQE adjust [niqe.ipynb](https://github.com/goutern/imagedeblur/blob/main/niqe.ipynb "niqe.ipynb") to reference your deblured images. Please note if you have a lot of images this may take a long time and use a lot of memory.

## Results
![BRISQUE](/results/BRISQUE.jpg?raw=true)
![NIQE](/results/NIQE.jpg?raw=true)
![RESULTS](/results/zoomed_in_results.jpg?raw=true)

We have also included sample results for each method located in:
- Original Frames: results/original
- GFN: results/gfn
- GAN: results/gan
- Motion: results/motion
- Out of focus: results/out-of-focus



## Python Dependencies

libsvm
svm
numpy
cv2
scipy
matplotlib



## Citations
- [1] Mittal, Anish & Moorthy, Anush & Bovik, Alan. (2012). No-Reference Image Quality Assessment in the Spatial Domain. IEEE transactions on image processing : a publication of the IEEE Signal Processing Society. 21. 10.1109/TIP.2012.2214050. 
- [2] Mittal, Anish & Soundararajan, Rajiv & Bovik, Alan. (2013). Making a “Completely Blind” Image Quality Analyzer. Signal Processing Letters, IEEE. 20. 209-212. 10.1109/LSP.2012.2227726. 



