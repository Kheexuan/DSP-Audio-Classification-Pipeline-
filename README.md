# DSP Audio Classification Pipeline
# Introduction
The objective of this project is to develop a complete audio classification pipeline by applying digital signal processing techniques to audio data. These techniques are used to clean and preprocess the audio signals, reduce noise and unwanted distortions, and extract meaningful features that can better represent the important characteristics of the sound.

This is important because raw audio data often contains irrelevant information such as silence, background noise, and inconsistent signal quality, which can affect the learning process of the model. By improving the quality and consistency of the input data, the model can focus more on the actual sound patterns that are useful for classification. In addition, extracting suitable audio features helps transform the raw waveform into a more informative representation, making it easier for the model to distinguish between different sound classes. Overall, the project aims to improve the performance, accuracy, and reliability of the audio classification model.

# Understanding of Data
In order to process the data effectively, the first and most crucial step is to develop a thorough understanding of the data itself. Before any preprocessing, feature extraction, or modelling can be carried out, it is important to examine the nature of the data, its structure, its quality, and the patterns it contains.

## Empty Gap Between Audio
<p align="center">
  <img width="600" alt="image" src="https://github.com/user-attachments/assets/4cd0b230-4139-4bdf-8572-c3bdbdc1608c" />
</p>
The first issue identified is the presence of empty spaces in the audio data. These silent sections not only increase the training duration by introducing unnecessary data, but may also be interpreted by the model as meaningful information, which can negatively affect its learning performance.

## Noisy Background

<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/cafd193f-701f-454f-8afd-851bc32555a4" />
</p>

The second issue identified was the presence of noise within the dataset. In many of the audio samples, the meaningful signal was concentrated only within a short duration, for the example wave the its approximately 1.2 to 1.8 seconds, while the remaining portions of the waveform were largely filled with white noise. This means that a significant part of each audio clip did not contain useful information for the classification task. As a result, the extracted features may be influenced by irrelevant noise rather than the actual signal of interest, which can reduce the model’s ability to learn accurate and discriminative patterns.


## Noisy Like Audio
<p align="center">
<img width="600" alt="image" src="https://github.com/user-attachments/assets/78416ef9-dd5b-4f0e-899f-965b33758c8c" />
</p>
Lastly, the waveform introduces a broader classification challenge due to the nature of the sounds it contains. In the example above, the audio sample consists of motor sounds. In typical audio processing tasks, particularly in speech recognition, such sounds are often regarded as noise and are commonly suppressed during preprocessing. However, in this classification task, the same treatment cannot be applied, as the motor sound is not noise but the key signal that must be analysed.

This means that standard noise suppression techniques used in other audio domains may not be directly suitable, since they risk removing features that are essential for classification. As a result, the preprocessing strategy must be carefully adapted to the task so that irrelevant background interference is reduced without compromising the meaningful acoustic properties of the motor sound.

# Date Processing
## Audio Resampling
The audio first needs to be resampled to 16 kHz. This is an important step because it ensures that all audio samples are represented using the same sampling rate and follow a consistent structure, which makes them easier to process, compare, and analyse. If the audio files have different sampling rates, the same sound may be represented differently across samples, which can introduce inconsistency into the dataset and affect the reliability of later processing steps.

## Trim Silence
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

## Noise Reduction
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

## Noisy Like Audio Processing
For noise reduction to be more effective, it was necessary to distinguish between ordinary background noise and actual noise-like audio classes. The idea was that sounds such as motors or other consistent mechanical sources, which contain more stable frequency components, would have lower spectral flatness than sounds with more dynamic and varying audio content. Based on that analogy, noise reduction was applied only yo audio samples with low flatness values, as there were more likely to represent steady , noise like rather than irregular or complex audio.
## Root Mean Sqaure Normalisation Over Peak Normalisation
Root Mean Sqaure( RMS) was the better fit here as it adjusted the audio levels based on overall enegry of the signal rather than only affecting the highest amplitude values. RMS can resulty in a more consistent representation of perceived loudness.

## DC offset
DC offset is generally good to perform every single time. DC offset correcttion is performed to remove any constant bias in the aduio signal that shifts the waveform away from zero. This bias does not represent meaning full content and can interfere with signal processing task. Removing the DC offset ensures that the waveform is properly centred, allowing the extracted audio features to reflect the true characteristic of the sound more accurately.

## Pre-emphasis Filter Was Considered but Not Applied
Pre-emphasis filter was considered but not ulterlize becuase the model was designed to classify many different types of sounds with varying charactertistic.
# Additional Processing
This chapter was developed as additional findings emerged throughout the process, particularly when the model’s validation results were obtained.

<p align="center">
<img width="554" height="697" alt="image" src="https://github.com/user-attachments/assets/30da5204-ee9c-4837-9459-9d36def4e845" />
</p>
It was observed that the system was unable to detect airplane and helicopter sounds effectively. These sound types are generally more noise-like and less structured than other audio classes, which may make them harder for the model to distinguish. This limitation led to further investigation into the characteristics of these sound categories in order to better understand why the model struggled and whether different features or approaches were needed to represent them more accurately.

<p align="center">
<img width="806" height="473" alt="image" src="https://github.com/user-attachments/assets/655592bf-7af6-46ae-8b3f-4ed690454380" />
</p>
To better understand the audio, examining only the spectrogram and time-domain waveform was not sufficient, as these representations did not reveal a very clear distinguishing characteristic for this type of sound. This is because most of the frequency range tends to be activated, making the audio appear broad and noise-like rather than showing a distinct pattern.

To address this, key audio features were extracted for further analysis such as the energy level and flatness. From this, it was observed that such sounds typically exhibit very low spectral flatness values, often below 0.01, and are mainly concentrated in the lower frequency range of approximately 0 to 600 Hz, as shown in the figure above.

<p align="center">
<img width="1243" height="484" alt="image" src="https://github.com/user-attachments/assets/cff048bc-7d25-48e1-bdb6-53da3a3a86c4" />
</p>
To confirm the analysis, the same observations were examined across both helicopter and hand saw audio samples, as these sound types also exhibit relatively noisy characteristics. As shown in the figure above, the pattern remains generally consistent for both classes: the spectral flatness stays low, typically below 0.1, although it is still slightly higher than that of aeroplane sounds. 

In addition, most of the energy remains concentrated in the lower frequency range, with only a few higher frequency outliers. This suggests that these noisy sound classes share similar low-frequency and low-flatness characteristics, even though the exact distribution may vary slightly between them.

## Low Pass Filter
<p align="center">
<img width="350" height="672" alt="image" src="https://github.com/user-attachments/assets/5816ac08-db20-41f5-b0c0-22eda687ea69" />
</p>
After these characteristics were identified, a low-pass filter was implemented to focus on frequencies below 1000 Hz, together with a threshold of 0.5 and a spectral flatness condition of less than 0.005. As shown in the figure above, this led to a significant improvement in both readings, as the system was now able to identify the sound correctly, whereas previously it had failed to detect it. This suggests that constraining the analysis to the lower-frequency region and applying stricter flatness-based conditions helped the model better capture the acoustic properties of this sound type.

# Features Extraction
https://librosa.org/doc/latest/feature.html

Below are some of the functions available in librosa, arranged to support the decision-making process on which features should be extracted.

| Types Of Extraction | Design | Selected |
|------|------|------|
| MFCC | Easy for Machine Learning | Selected |
| Spectral Contrast | Emphasis Energy Difference | Selected |
| Spectral Centroid | Speading in Spectrum | Selected |
| Spectral Bandwidth | Emphasis Spreading in Spectrum | Selected |
| Spectral Rolloff | Below What Frequency does Most Energy  | Selected |
| Spectral Flatness | Sound more noisy or tone  | Selected |
| Zero_Crossing | Times the signal crosses zero  | Selected |
| Root Mean Square | Audio Segmentation  | Selected |
| Log Spectrogram | Raw Spectrogram  | Selected |
|------|------|------|
| Log Melspectrogram | Emphasis On Human Hearing  | Not Selected |


## MFCC:
Mel-Frequency Cepstral Coefficients (MFCCs) are widely used in audio and speech machine learning tasks because they provide a compact representation of the sound signal. Although MFCCs may not be the easiest for humans to interpret visually, they are highly effective for machine learning models as they capture the most important spectral characteristics of audio. For this reason, MFCC was selected as one of the key features.

## Spectral Contrast:
Spectral Contrast measures the difference in energy between peaks and valleys in the frequency spectrum. This helps describe the richness and texture of a sound by showing how much variation exists across different frequency bands. It was selected because this contrast can be useful in distinguishing between different types of audio signals.

## Spectral Contrast:
Spectral Centroid indicates where the centre sterngth of the spectrum lies, giving an idea of how bright or sharp a sound is. A higher centroid usually means the sound contains more high-frequency components, while a lower centroid suggests a darker or deeper sound. It was selected because it is useful in representing the overall distribution of energy in the spectrum.

## Spectral Bandwidth:
Spectral Bandwidth measures how spread out the frequencies are around the spectral centroid. In other words, it shows the range of frequencies present in the sound and whether the energy is concentrated or dispersed. It was selected because this spreading information helps describe the complexity and texture of the audio signal.

## Spectral Rolloff
Spectral Rolloff refers to the frequency below which most of the signal’s energy is concentrated. It is useful for identifying whether a sound is dominated by lower or higher frequencies. This feature was selected because it provides a simple but effective way to represent the distribution of spectral energy.

## Spectral Flatness
Spectral Flatness measures whether a sound is more noise-like or tone-like. A higher flatness value suggests the signal is more similar to noise, while a lower value indicates a more tonal or harmonic sound. It was selected because this feature helps differentiate between structured sounds and noisy signals.

## Zero Crossing Rate
Zero Crossing Rate counts how many times the audio signal crosses the zero amplitude line within a given time frame. This feature is often used to capture the noisiness or sharpness of a signal, as noisier sounds tend to cross zero more often. It was selected because it provides a simple and useful representation of signal behaviour over time.

## Root Mean Square
Root Mean Square (RMS) measures the average energy or loudness of an audio signal. It is commonly used in audio segmentation because it helps identify changes in intensity, such as silence, speech, or other sound events. It was selected because energy information is important for understanding the structure and dynamics of audio.

## Log Spectrogram Over Log Mel Spectrogram
The log spectrogram was selected over Mel is because the audio data in this project is not strongly dependent on speech-specific characteristics, where Mel-scaled features are usually most beneficial. The Mel scale is designed to place emphasis on human hearing, rather than preserving the full raw frequency detail of the signal. 

In addition, MFCCs were already included as a selected feature, and since MFCCs are themselves derived from the Mel spectrogram, using both could introduce overlapping information. Therefore, the log spectrogram was chosen instead because it provides a more direct and raw representation of the original spectral content, allowing the model to learn from less compressed frequency information.

# Comparison Between Models
The extracted features were tested on 3 different model , SVM , logistic linearity 

# Configuration and Settings
# Conclusion
