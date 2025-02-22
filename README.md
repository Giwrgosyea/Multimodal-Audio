# Multimodal-Audio

At this assignment we will create a model that used for modal speech emotion recognition on YouTube interviews. As first step we crawled videos that contain English dialogs from YouTube. After that, 3 sentiments classifiers used to recognize negative, neutral and positive sentiments. The most confident of these decisions (negative, neutral and positive sentiments) used to train an audio-based emotion recognizer. As next step, this trained audio model will be used to classify other testing videos. At the end we are going to evaluate the model by defining the ground truth. A subset of the audio classifier outcomes, specifically 100 samples of negative, neutral and positive were evaluated by listening them and put a sentiment tag. After these 2 different tags are compared.


Dataset.csv
The file of the format is:
Id,URL,Title,Audio,Captions,Polarity,Pickle
1,https://www.youtube.com/watch?v=zkpelP3x0mw,Westley Allan Dodd - Last Interview before Execution,audio/1.wav,subtitles/1.srt,polarity_csv/polarity_1.csv,pickle_lists/polarity_1.p
We have defined a unique Id for each video. Mp3, wav, srt and vtt files will be saved with the VideoId and the appropriate extension
	__________________________________________________________________________________
	__________________________________________________________________________________
	
	
	
	*************************************************************************************************
******************************COMMANDS TO RUN THE SCRIPTS:******************************

python Downloader.py 
python Parser_caption.py 
python Parser_audio.py <pyaudioanalysis_path>

Define the variable pyaudioanalysis_path, the local path of the pyaudio analysis repo
	*************************************************************************************************	
	
First run Downloader.py. The functionality of this script is:

_________________________________________________________________________________________
	1. download all captions from youtube with youtube-dl, urls are listed in dataset.csv  (At that moment use only 5 videos in order to take some first results)
 2. subtitles will be downloaded at /subtitles subfolder
	3.  mp3 will be downloaded at /audio subfolder
	4. convert from vtt format to srt inside /subtitles subfolder
	5. Conver them to wav inside /audio subfolder
	__________________________________________________________________________________
	__________________________________________________________________________________
	
	
	
Afer Run the script Parser_caption.py The functionality of this script is:
_______________________________________________________________
1. Take each srt file. Remove tags, find: start and end for each sentence (caption are splitted in time segments, if segment is less than 2 secs exclude it)
2. Find the polarity of each segment. Remove segments with no text, eg: [Laughing]
3. text-blob and Vader are used for sentiment analysis. At the end, the average of these 2 classifiers used to characterize a segment as positive/negative/neutral
5. Make a csv file for each srt file that contains (time segment, pollarity). The name of the file will be:pollarity_videoId.csv, eg:polarity_0.csv and will be saved at /polarity_csv subfolder
6. Make a pickle file for each srt wiht the same context of polarity_videoId.csv file. Save them at the subfolder /pickle_lists.
_______________________________________________________________________________
______________________________________________________________________________



Afer Run the script Parser_audio.py The functionality of this script is:
_______________________________________________________________
1. Take each wav file (from subfolder audio) and split audio in segments, The duration of segments based on the caption segmnets.
2. For the segments read the polarity files inside /pickle_lists subfolder. 
3. After at each one of these segments assign the polarity that has been extracted from sentiment analysis
4. Create 3 subfolders inside audio: 
    Positive
    Negative
    Neutral
   and add the correlated segments according their polarity.
5. Use these 3 subfolders(positive,negative,neutral) as groundtruth and train a SVM classifier from pyaudioAnalysis (featureAndTrain)

_______________________________________________________________________________
______________________________________________________________________________

#INSTALL YOUTUBE-DL and UPDATE it EVERY DAY#
sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl

#INSTAL PYAUDIO-ANALYSIS#
pip install numpy matplotlib scipy sklearn hmmlearn simplejson eyed3 pydub
git clone https://github.com/tyiannak/pyAudioAnalysis.git
pip install -e .
