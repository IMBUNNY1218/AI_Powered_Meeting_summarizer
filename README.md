# AI_Powered_Meeting_summarizer

We have used "Wispher" for extracting the text from the audio file, and diarization. 
We used opensource "AI GPT for all" which is an offline GPT used for the summarization of the meeting and also highlighting the key points.
"Trello API" is also involved to display the tasks and to mannage them using the trello website by creating boards and cards from the summary of the meeting

Run all the codes in the Google Colab. The codes are running on single core CPU not GPU, so it takes time to run.
Install these libraries to run the code without any possible errors

To list these as required dependencies in your GitHub README file, you can create a **"Requirements"** section and provide installation instructions. Here's how you can format it:  

### Example README Section:

```markdown
## Requirements

To run this project, you need to install the following dependencies:

### System Dependencies:
Ensure you have `ffmpeg` installed:
```bash
sudo apt-get install -y ffmpeg
```

### Python Dependencies:
Install the required Python packages using pip:
```bash
pip install torch torchaudio
pip install git+https://github.com/m-bain/whisperX.git
pip install ffmpeg-python
pip install gpt4all
pip install py-trello
```

Alternatively, you can install all dependencies using a single command:
```bash
pip install -r requirements.txt
```

## Installation using `requirements.txt`
You can create a `requirements.txt` file and add:
```
torch
torchaudio
git+https://github.com/m-bain/whisperX.git
ffmpeg-python
gpt4all
py-trello
```
Then install all dependencies with:
```bash
pip install -r requirements.txt
```
```

This ensures that anyone cloning your repository can easily install the necessary dependencies. Let me know if you need further refinements! 🚀




Then the text is given to "gpt4all" and it is summarized using the power of AI, the points that are obtained from the meeting are sent to trello board for better understanding using the "Trell API".
(Note:The code runs for almost 8-10 min, so do not disturb the code if you do not get the response immedietly)
The process will be carried out by the following code: 


