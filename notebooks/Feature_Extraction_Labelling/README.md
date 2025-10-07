# **Feature Extraction Pipeline**

This script automates the process of converting raw audio clips into a structured, model-ready dataset of Mel spectrograms. It takes a directory of sorted audio file chunks (audio segments) and outputs individual feature files (.npy) and a master index file (metadata.csv) containing paths to each file with it's corresponding label.

This approach is memory-efficient, allowing it to process thousands of audio files without running out of RAM.

## **Workflow ⚙️**

Setup: Defines the source directory for processed (segmented) audio clips, a destination directory for the new feature files, and a numerical mapping for the language labels.

Iterate and Read: It recursively navigates through the source directories, loading each individual .wav audio chunk one by one.

Feature Extraction: For each audio clip, it performs two key operations using the librosa library:

>Generates a Mel spectrogram, a 2D representation of the audio that is ideal for speech analysis.

>Converts the spectrogram's amplitude to a decibel (dB) scale for better perceptual representation (human understanding).

Enforce Uniform Shape: Every spectrogram is resized to a fixed shape of (128, 256) (height x width).

>If a spectrogram is too long, it is trimmed.

>If it is too short, it is padded with zeros (representing silence).

Save to Disk: Each processed spectrogram is immediately saved to the features directory as its own unique .npy file. This prevents memory overload by not holding all features in RAM at once.

Create Metadata Index: As each feature file is saved, its path and corresponding numerical label are recorded. After all files are processed, this index is saved as a metadata.csv file, which serves as the master guide to the dataset.

## Usage
1.	Ensure your sliced audio files are organized into sub-folders by language within the Data/processed_audio directory.
2.	Update the paths at the top of the script if your directory structure is different.
3.	Run the Python script. It will create a features folder and a metadata.csv file containing your complete, model-ready dataset.
   
## Dependencies
* librosa
* numpy
* pathlib
* csv
