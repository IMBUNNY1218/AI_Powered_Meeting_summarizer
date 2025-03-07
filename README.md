# AI_Powered_Meeting_summarizer

We have used "Wispher" for extracting the text from the audio file, and diarization. 
We used opensource "AI GPT for all" which is an offline GPT used for the summarization of the meeting and also highlighting the key points.
"Trello API" is also involved to display the tasks and to mannage them using the trello website by creating boards and cards from the summary of the meeting

Run all the codes in the Google Colab. The codes are running on single core CPU not GPU, so it takes time to run.
Install these libraries to run the code without any possible errors

!apt-get install -y ffmpeg  # System-level ffmpeg for audio extraction
!pip install torch torchaudio
!pip install git+https://github.com/m-bain/whisperX.git
!pip install ffmpeg-python
!pip install gpt4all
pip install gpt4all
!pip install torch torchaudio
!pip install git+https://github.com/m-bain/whisperX.git
!pip install ffmpeg-python
!apt-get install -y ffmpeg
pip install py-trello


The speech is extracted in text format using whisper.




Then the text is given to "gpt4all" and it is summarized using the power of AI, the points that are obtained from the meeting are sent to trello board for better understanding using the "Trell API".
(Note:The code runs for almost 8-10 min, so do not disturb the code if you do not get the response immedietly)
The process will be carried out by the following code: 


