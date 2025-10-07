# **Audio Preprocessing - Resampling and Slicing Pipeline**
## **Project Overview**

This notebook is part of the Naija-Language-ID Project, which aims to build an AI system that can detect Nigeria’s major local languages — Hausa, Igbo, and Yoruba  
from short audio clips. This stage of the project focuses on automated preprocessing of large raw audio files:
* Detecting regions of human speech using Silero VAD (Voice Activity Detection).
* Resampling all audio to 16 kHz mono for consistency with speech models.
* Slicing each language’s long recordings into clean, speech-only segments for training.

## **Key Steps in the Pipeline**
1. Install and all the required libraries
* silero-vad for speech detection
* pydub for audio slicing
* soundfile for efficient audio streaming in chunks
* torchaudio for resampling
* pathlib and numpy for file management and data handling

2. Define Source and Destination Paths
* Source paths: point to the raw .wav recordings stored in Google Drive (one folder per language).
* Processed paths: define where the cleaned, sliced audio segments will be stored.
* The helper function define_source_paths() automatically lists all the files in each source directory.

3. Load Silero VAD Model
The pre-trained Silero VAD model is downloaded from PyTorch Hub:

model, silero_utils = torch.hub.load('snakers4/silero-vad', 'silero_vad')

It detects speech segments in real-time audio streams by returning timestamp intervals where human speech occurs.

4. Process and Resample Audio
   
The core function resample_and_processing() performs:
* Chunked streaming using soundfile — processes 30 seconds of audio at a time to avoid memory overload.
* On-the-fly resampling with torchaudio.transforms.Resample to 16kHz.
* Speech detection using Silero VAD to extract start–end timestamps of spoken parts.
* Offset adjustment to align timestamps across all chunks for accurate slicing.
* Precision slicing with pydub.AudioSegment using millisecond-based indexing.
* Exporting clean speech clips into structured folders (processed_audio/spoken_<language>).

5. Run the Function

The function is executed separately for Igbo, Hausa, and Yoruba:

resample_and_processing(source_dir=igbo_source_path, silero_model=model, ...)

Each language folder is processed independently and saved to its corresponding destination.

6. Generate Requirements

Finally, all package dependencies are saved into a requirements.txt file for reproducibility:

!pip freeze > requirements.txt

## **Why This Pipeline Works**
* Efficient: Streams audio in chunks instead of loading huge files into memory.
* Accurate: Uses a pre-trained, PyTorch-based speech detector fine-tuned for real-world voice data.
* Organized: Automatically sorts and saves all segments into structured folders using pathlib.
* Consistent: Every output file is resampled to 16 kHz mono, ready for feature extraction and model training.
