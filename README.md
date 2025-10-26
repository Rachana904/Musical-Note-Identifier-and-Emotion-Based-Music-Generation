# Musical-Note-Identifier-and-Emotion-Based-Music-Generation
This project leverages deep learning to generate musical pieces that evoke specific emotions. By analyzing an input audio file (from a local MP3 or a YouTube link), the system identifies its musical properties and then composes new music in a "happy" or "sad" style. The core of the project is a pair of Recurrent Neural Networks (LSTMs) trained on the EMOPIA dataset, each specialized in generating music with a distinct emotional valence.
🎵 Features
Audio Input Flexibility: Accepts audio from both local MP3 files and YouTube URLs.
Musical Analysis: Converts input audio into MIDI format to analyze and extract its unique musical notes and tempo.
Emotion-Driven Generation: Utilizes two distinct LSTM models, one trained on "happy" music and the other on "sad" music, to generate new compositions.
Customizable Output: The generated music is tailored with emotion-specific characteristics, such as tempo, note duration, and velocity.
Multiple Output Formats: Produces a playable WAV audio file, a downloadable MIDI file, and a PDF of the musical score for the generated piece.
⚙️ How It Works
The project follows a sequential pipeline from audio input to musical output.
1. Model Training (Happy & Sad Models)
Two separate Long Short-Term Memory (LSTM) models were trained to learn the patterns associated with happy and sad music.
Dataset: The EMOPIA dataset, which contains a collection of MIDI files with emotional annotations, was used for training. The dataset was filtered to create two subsets: one for "happy" pieces (quadrants 1 and 4) and one for "sad" pieces (quadrants 2 and 3).
Data Preparation:
MIDI files from each emotion subset were parsed using the music21 library to extract sequences of musical notes and rests.
These sequences were then converted into integer representations.
A sliding window approach was used to create input sequences of a fixed length (100 notes) and their corresponding target note (the next note in the sequence).
Model Architecture: A Sequential model was built using TensorFlow/Keras with the following architecture for both the happy and sad models:
LSTM(256, return_sequences=True): The first LSTM layer processes the input sequence.
Dropout(0.3): A dropout layer to prevent overfitting.
LSTM(256): A second LSTM layer to further learn the temporal dependencies.
Dropout(0.3): Another dropout layer.
Dense(num_unique_notes, activation='softmax'): A final dense layer that outputs a probability distribution over all possible unique notes.
Training: Each model was trained on its respective dataset ("happy" or "sad"). ModelCheckpoint was used to save the best-performing model based on validation loss.
2. Music Generation Pipeline
The main script orchestrates the generation process based on user input.
Input Processing:
The user provides a YouTube URL or uploads an MP3 file.
yt-dlp is used to download audio from YouTube.
pydub converts the input MP3 into a WAV file.
Note Identification:
librosa analyzes the WAV file to detect the tempo and onsets of musical notes.
The pitch of each note is identified, and the entire piece is transcribed into a MIDI file using pretty_midi.
Emotion Selection: The user chooses to generate either "happy" or "sad" music.
AI-Powered Composition:
The corresponding pre-trained model (happy_model.keras or sad_model.keras) is loaded.
A random sequence of notes is chosen to "seed" the generation process.
The model predictively generates a sequence of new notes, one at a time. A temperature sampling function is introduced to control the randomness and creativity of the generated music.
Emotion-specific parameters are applied:
Happy Music: Generated with a faster tempo (120 BPM) and higher note velocity for a more energetic feel.
Sad Music: Generated with a slower tempo (70 BPM), longer note durations, and softer velocity to create a more legato and melancholic effect.
Output Synthesis:
The generated sequence of notes is converted into a MIDI file using music21.
FluidSynth synthesizes this MIDI file into a high-quality WAV audio file for playback.
MuseScore is used to render the musical notation into a downloadable PDF sheet.
🛠️ Technologies & Libraries
This project is built upon a foundation of powerful libraries for deep learning, audio processing, and music theory.
Deep Learning: TensorFlow, Keras
Music & MIDI Processing: music21, pretty_midi, librosa
Audio Handling: pydub, scipy, yt-dlp
Audio Synthesis & Notation: FluidSynth, Timidity, MuseScore
Data Handling: NumPy, Pandas
🚀 Setup and Installation
This project is designed to be run in a Google Colab environment to handle the dependencies and computational requirements.
Clone the Repository:
code
Bash
git clone https://github.com/your-username/emotion-based-music-generation.git
cd emotion-based-music-generation
Upload to Google Drive:
Upload the entire project folder to your Google Drive.
Ensure the pre-trained models (happy_model.keras, sad_model.keras) and metadata files (happy_metadata.pkl, sad_metadata.pkl) are in the correct directory as referenced in the main notebook.
Open in Google Colab:
Open the music_generation.ipynb notebook in Google Colab.
Install Dependencies:
The notebook contains cells at the beginning to install all the necessary pip packages and apt-get libraries. Simply run these cells to set up the environment.
Usage
Run the Main Notebook: Execute the cells in the music_generation.ipynb notebook sequentially.
Provide Input: You will be prompted to choose between uploading an MP3 file or providing a YouTube URL.
If you choose mp3, an upload dialog will appear.
If you choose url, you will be asked to paste a YouTube link.
Choose an Emotion: You will then be prompted to enter happy or sad to select the desired emotion for the generated music.
Get the Output: The notebook will process the input, generate the music, and provide the following outputs:
An embedded audio player to listen to the generated WAV file.
A download link for the generated musical sheet in PDF format.
