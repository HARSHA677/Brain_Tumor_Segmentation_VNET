# Brain_Tumor_Segmentation_VNET


# About Dataset
*Data released with multimodal brain tumor segmentation challenge by Medical Image Computing and Computer Assisted Intervention (MICCAI) is used for training.

*3D dataset consists of pre surgical MRI scans of 210 High-Grade Glioma (HGG) patients and 75 Low-Grade Glioma (LGG).

*The dataset has 4 modalities- T1-weighted (T1w), post-contrast T1-weighted (T1ce), T2-weighted (T2), Fluid Attenuated Inversion Recovery (FLAIR).

*Ground truth (seg) is taken as the target output

*These images are manually segmented by expert neuroradiologist labelled as as enhancing tumor (label 4), peritumoral edema (label 2), and the core (label 1)

*Presence of Multiple tumor region is more visible with HGG than LGG- thus only HGGs are used for training.

*The HGG consists of 210 patients data. The deep learning models were trained in the batches of 50 patients due to limitations of Google Colaboratory.

*Only 30 samples of the HGG dataset considered for training due to memory (RAM and GPU) constraint.


# Pre-Processing
*Each 3D volume of 4 has a size of 240×240×155 for all modalities and ground truth.

*After slicing we will get 155 2D images of size 240×240 size.

*Only 90 slices (slice no 30 to slice no 120) from each volume are selected.

*These slices cropped to 192×192 (Background noise) 18,900 2D images of each MRI modality. (210 HGG data × 90 slices = 18900)

*Total images for all modalities will become 75,600. (210 HGG patient's data × 90 slices of each modality × 4 modalities = 75,600 2D images)


# Feature Engineering
Feature engineering involves transforming raw data into a format that is suitable for modeling

*One Hot Encoding: The ground truth labels are typically categorical, representing different tissue types (e.g., tumor core, edema, non-enhancing tumor core, background). One-hot encoding converts these categorical labels into binary vectors where each element corresponds to a class label, and only one element is 1 (indicating the presence of that class) while the rest are 0

*Normalization:Normalization is often applied to standardize the intensity values across different images or imaging modalities. This is important because the intensity range can vary between scans and modalities, and normalizing the data helps ensure that the model learns meaningful patterns rather than being biased by differences in intensity.

Normalization approach used here is to subtract the mean and divide by the maximum intensity value. This centers the data around zero and scales it to a range between -1 and 

Future Approach (Restricted by Memory issue)

Data augmentation is indeed a powerful technique in feature engineering, particularly in computer vision tasks like medical image analysis.but is computationally expensive and memory-intensive, especially when dealing with large datasets or high-resolution images. restricted that approach.

# Train-Test Split
*Selecting specific slices[:,30:120,30:222,30:222,:] from each sample and reshaping the data accordingly, you're able to obtain 1620 samples with a shape of 192x192x4 for training from the original 30 samples with four channels.

*Data split into 3:1:1- 60% images for training, 20% images for testing, and 20% for validation. All the models are trained with batch size-8 and no of epochs-30.

# Model-2D VNET Architecture
*While Preprocesing 3D images are preprocessed to 2D slices and there by we are using 2D convolution layers and thereby reduce educes computational complexity and memory requirements and high chances to lead to overfitting(more parameters in 3D), especially when the training dataset is limited.

# Components
Convolutional Block (conv_block): Used in both the encoder and decoder.

Up-Convolutional Block (up_conv_block): Used in the decoder.

Skip Connections: Used to connect encoder blocks to decoder blocks.

# Parametric Rectified Linear Unit (PReLU)
*PReLU helps alleviate the issue of dying ReLU neurons, where neurons become inactive (i.e., output zero) for negative inputs during training. By allowing a small negative gradient for negative inputs and allowing for more flexibility in modeling complex relationships in the data.

# Batch Normalisation
*During training, batch normalization calculates the mean and variance of each feature within the mini-batch.

*These statistics are then used to normalize the activations.

*It ensures that the activations within each layer stay within a stable range, making it easier for the network to learn.

*Thus,for each feature (neuron), we subtract the mean and divide by the standard deviation across a mini-batch of data.

*Acts a regularisastion technique.

# Dice Coeffcient
Measures the overlap between the predicted zone and the ground truth as a proportion, so the size of the class is taken into account.*
*Consider spatial overlap and prioritize it over pixel-wise discrepancies to enhance spatial learning effectiveness.

*By considering the intersection of the two segmentation sets relative to their total sizes, the Dice coefficient provides a concise measure ranging from 0 to 1, where 1 indicates perfect spatial overlap and 0 indicates no overlap. This metric is especially valuable in analyzing complex spatial relationships, where objects may vary in size, shape, and orientation, as it focuses on overall spatial agreement rather than pixel-wise differences

*Models trained with no of epochs-30.

# Model Evaluation
*The training and validation loss, as well as the dice coefficient, were plotted against the number of iterations. This technique visually represents the trend of these metrics over the course of training, offering insights into the model's performance and convergence

*A table was generated to summarize the results for the training, validation, and testing datasets. This table included metrics such as loss, dice coefficient, and accuracy for each dataset, providing a concise comparison of the model's performance across different evaluation stages.



#Result

<img width="799" alt="Screenshot 2024-05-05 at 9 51 51 PM" src="https://github.com/HARSHA677/Brain_Tumor_Segmentation_VNET/assets/53110311/5ea9de15-3dbe-4887-969f-ed2b8bbdb480">


![image](https://github.com/HARSHA677/Brain_Tumor_Segmentation_VNET/assets/53110311/39fe5f02-1ae8-452c-baf6-8f3d57ac00de)


*In conclusion, the performance metrics of model showcase its robustness and effectiveness in the segmentation/classification task.

*Specifically, the dice coefficient scores of 0.997, 0.996, and 0.995 for training, validation, and test sets respectively indicate a high degree of overlap between predicted and actual values, demonstrating the model's proficiency in accurately segmenting/classifying data.

*Additionally, with accuracy scores consistently above 99.5% across all datasets, our model exhibits strong predictive capabilities and generalization to unseen data.

Furthermore, the low loss values, ranging from 0.0027 to 0.0048, signify that*the model effectively minimizes errors during training, ensuring reliable performance in real-world applications. These results underscore the efficacy and reliability of our model for the intended task.







