# DSP Audio Classification Pipeline
# Introduction
The objective of this project is to develop and set up a complete audio classification pipeline by applying digital signal processing techniques to audio data. The purpose of using these techniques is to clean and preprocess the audio signals, reduce noise or unwanted distortions, and extract meaningful features that can better represent the important characteristics of the sound. By improving the quality of the input data and selecting relevant audio features, the project aims to enhance the overall performance, accuracy, and reliability of the classification model.
# Understanding of Data
In order to process the data effectively, the first and most crucial step is to develop a thorough understanding of the data itself. Before any preprocessing, feature extraction, or modelling can be carried out, it is important to examine the nature of the data, its structure, its quality, and the patterns it contains.

<p align="center">
  <img width="600" alt="image" src="https://github.com/user-attachments/assets/4cd0b230-4139-4bdf-8572-c3bdbdc1608c" />
</p>
The first issue identified is the presence of empty spaces in the audio data. These silent sections not only increase the training duration by introducing unnecessary data, but may also be interpreted by the model as meaningful information, which can negatively affect its learning performance.

<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/cafd193f-701f-454f-8afd-851bc32555a4" />
</p>

The second issue identified was the presence of noise within the dataset. In many of the audio samples, the meaningful signal was concentrated only within a short duration, for the example wave the its approximately 1.2 to 1.8 seconds, while the remaining portions of the waveform were largely filled with white noise. This means that a significant part of each audio clip did not contain useful information for the classification task. As a result, the extracted features may be influenced by irrelevant noise rather than the actual signal of interest, which can reduce the model’s ability to learn accurate and discriminative patterns.

<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/78416ef9-dd5b-4f0e-899f-965b33758c8c" />
</p>
Lastly, the waveform introduces a broader classification challenge due to the nature of the sounds it contains. In the example above, the audio sample consists of motor sounds. In typical audio processing tasks, particularly in speech recognition, such sounds are often regarded as noise and are commonly suppressed during preprocessing. However, in this classification task, the same treatment cannot be applied, as the motor sound is not noise but the key signal that must be analysed.

This means that standard noise suppression techniques used in other audio domains may not be directly suitable, since they risk removing features that are essential for classification. As a result, the preprocessing strategy must be carefully adapted to the task so that irrelevant background interference is reduced without compromising the meaningful acoustic properties of the motor sound.

# Date Processing
The audio first needs to be resampled to 16 kHz. This is an important step because it ensures that all audio samples are represented using the same sampling rate and follow a consistent structure, which makes them easier to process, compare, and analyse. If the audio files have different sampling rates, the same sound may be represented differently across samples, which can introduce inconsistency into the dataset and affect the reliability of later processing steps.


<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/f7b42700-809c-4917-b128-1fd8d1e2d323" />
</p>

Next, the issue of empty durations within the audio signal was addressed. In some samples, such as dog sounds or fireworks, there were silent or low-energy sections before, after, or between the actual sound events. To handle this, a silence trimming technique was used to remove these unnecessary gaps and retain only the more relevant portions of the signal.

This step is important because the empty sections do not contain useful information for the classification task, yet they increase the length of the audio and may introduce inconsistency across samples. By trimming these silent regions, the input becomes more focused on the actual sound of interest. As a result, the model is more likely to learn meaningful acoustic patterns rather than being influenced by uninformative empty spaces. This can improve both the efficiency of training and the overall quality of the learned representation.

However, a key difficulty lies in determining the appropriate threshold for silence trimming. Setting the decibel cut-off too high may lead to the removal of weaker yet meaningful parts of the signal, thereby discarding useful acoustic information required for classification. Conversely, setting the threshold too low would allow unnecessary silent or low-energy regions to remain, which limits the effectiveness of the trimming process.

As a result, the cut-off value must be selected carefully to achieve a balance between removing irrelevant silence and preserving important signal details. This highlights that silence trimming, while beneficial, must be adapted to the characteristics of the dataset to avoid negatively affecting model performance.

To determine a suitable threshold, the approach was to test the trimming method across a wider range of selected audio samples. One example was the fireworks audio, where the signal did not end immediately after the initial loud burst. Instead, the first pop was strong and was followed by a gradually fading sound. This decaying portion of the signal could also contain useful features for classification and therefore should not be removed too aggressively.

<p align="center">
<img width="600"  alt="image" src="https://github.com/user-attachments/assets/c923996a-0778-42f2-b7a0-1d18c08ab3fd" />
</p>
By testing different samples with varying sound patterns, it became possible to evaluate how the trimming threshold affected both the louder and softer parts of the signal. This was necessary in order to strike a balance between removing unnecessary silent regions and preserving the meaningful details of the audio, including weaker but still relevant components that occur after the main sound event. In the example above, the trimming threshold was set too aggressively, causing the cut to be too tight and capturing only a very short 0.5-second portion of the reverberation. As a result, part of the natural decay of the sound, which may contain useful information for classification, was lost.

<p align="center">
<img width="600"  alt="image" src="https://github.com/user-attachments/assets/5ca708eb-b188-42d4-ae72-0931575da167" />
</p>
The optimal threshold that was identified was around 33 dB. At this level, the signal was no longer excessively long or filled with very low-volume regions, while most of the important parts of the audio were still preserved. This provided a good balance between removing unnecessary silence and retaining meaningful sound information. As a result, the trimmed audio was reduced to around 2.5 seconds compared to the original 5-second clip, making the sample more compact while still capturing the key features needed for classification as seen from the above image.

<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/cafd193f-701f-454f-8afd-851bc32555a4" />
</p>

The next issue encountered in the dataset was the presence of strong background noise as seen on the iamge above, which could heavily affect the feature extraction process and reduce classification performance. Such noise may be incorrectly learned by the model as a meaningful feature, or it may mask the actual signal to the extent that accurate classification becomes difficult. In many cases, only the middle portion of the waveform contained the relevant audio information, while the surrounding regions were largely dominated by unwanted noise.

As a result, noise reduction became necessary to enhance the quality of the signal before training. By cleaning the audio and preserving the important section of the waveform, the model is better able to learn the relevant acoustic patterns, leading to more effective and reliable classification.

<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/6cd42ae1-9a2e-4461-acbe-8c66e593ec77" />
</p>
After noise reduction was applied, the changes in the waveform became much clearer in both the time-domain representation and the spectrogram. In the time-domain plot, the signal appeared cleaner and more focused, with less interference from unwanted background noise. Similarly, in the spectrogram, the red regions became more concentrated and better defined in specific areas, rather than being spread broadly across the image as seen previously.

This suggests that the noise reduction process was able to suppress a large portion of the irrelevant background noise while preserving the more meaningful components of the signal. In the earlier image, the red patches covered a much wider area, indicating that energy was dispersed more broadly and was likely influenced by noise. After preprocessing, the clearer and more localised red regions show that the important sound features became more distinguishable, which is beneficial for feature extraction and subsequent model training.


# Features Extraction
https://librosa.org/doc/latest/feature.html

Below are some of the functions available in librosa, arranged to support the decision-making process on which features should be extracted.

| Types Of Extraction | Design | Selected |
|------|------|------|
| Melspectrogram | Emphasis On Human Hearing  | Not Selected |
| MFCC | Easy for Machine Learning | Selected |
| Spectral Contrast | Emphasis Energy Difference | Selected |
| Spectral Centroid | Speading in Spectrum | Selected |
| Spectral Bandwidth | Emphasis Spreading in Spectrum | Selected |
| Spectral Rolloff | Below What Frequency does Most Energy  | Selected |
| Spectral Flatness | Sound more noisy or tone  | Selected |
| Zero_Crossing | Times the signal crosses zero  | Selected |
| Root Mean Square | Audio Segmentation  | Selected |




# Comparison Between Models
