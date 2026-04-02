# DSP Audio Classification Pipeline
# Introduction
The objective of this project is to develop and set up a complete audio classification pipeline by applying digital signal processing techniques to audio data. The purpose of using these techniques is to clean and preprocess the audio signals, reduce noise or unwanted distortions, and extract meaningful features that can better represent the important characteristics of the sound. By improving the quality of the input data and selecting relevant audio features, the project aims to enhance the overall performance, accuracy, and reliability of the classification model.
# Understanding of Data
In order to process the data effectively, the first and most crucial step is to develop a thorough understanding of the data itself. Before any preprocessing, feature extraction, or modelling can be carried out, it is important to examine the nature of the data, its structure, its quality, and the patterns it contains.

<p align="center">
  <img width="430" alt="image" src="https://github.com/user-attachments/assets/4cd0b230-4139-4bdf-8572-c3bdbdc1608c" />
</p>
The first issue identified is the presence of empty spaces in the audio data. These silent sections not only increase the training duration by introducing unnecessary data, but may also be interpreted by the model as meaningful information, which can negatively affect its learning performance.

<p align="center">
<img width="430" alt="image" src="https://github.com/user-attachments/assets/cafd193f-701f-454f-8afd-851bc32555a4" />
</p>

The second issue identified was the presence of noise within the dataset. In many of the audio samples, the meaningful signal was concentrated only within a short duration, for the example wave the its approximately 1.2 to 1.8 seconds, while the remaining portions of the waveform were largely filled with white noise. This means that a significant part of each audio clip did not contain useful information for the classification task. As a result, the extracted features may be influenced by irrelevant noise rather than the actual signal of interest, which can reduce the model’s ability to learn accurate and discriminative patterns.

<p align="center">
<img width="430" alt="image" src="https://github.com/user-attachments/assets/78416ef9-dd5b-4f0e-899f-965b33758c8c" />
</p>
Lastly, the waveform introduces a broader classification challenge due to the nature of the sounds it contains. In the example above, the audio sample consists of motor sounds. In typical audio processing tasks, particularly in speech recognition, such sounds are often regarded as noise and are commonly suppressed during preprocessing. However, in this classification task, the same treatment cannot be applied, as the motor sound is not noise but the key signal that must be analysed.

This means that standard noise suppression techniques used in other audio domains may not be directly suitable, since they risk removing features that are essential for classification. As a result, the preprocessing strategy must be carefully adapted to the task so that irrelevant background interference is reduced without compromising the meaningful acoustic properties of the motor sound.

# Date Processing
## Technique Used
Resampling 

## Validation
# Features Extraction

# Comparison Between Models
