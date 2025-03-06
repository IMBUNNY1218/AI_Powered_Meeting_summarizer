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

To extract the text form the audio file the following code will be used





import whisperx
device = "cpu"
model = whisperx.load_model("medium", device=device, compute_type="float32")
audio_file = "/content/extracted_audio.mp3"
transcription = model.transcribe(audio_file)
print("Transcription:", transcription)





Then the text is given to "gpt4all" and it is summarized using the power of AI, the points that are obtained from the meeting are sent to trello board for better understanding using the "Trell API".
(Note:The code runs for almost 8-10 min, so do not disturb the code if you do not get the response immedietly)
The process will be carried out by the following code: 



from gpt4all import GPT4All
import re
import json
from trello import TrelloClient
import os

# ============================================
# Assume transcription variable already exists
# Example transcription structure (from WhisperX):
# transcription = {
#     "segments": [
#         {"text": "Meeting started at 1 PM."},
#         {"text": "Discussion about project roadmap."}
#     ]
# }
# ============================================

# Extract full transcript text from WhisperX transcription
final_transcript = "\n".join(segment['text'] for segment in transcription['segments'])

# ============================================
# Load GPT4All Model (Ensure model file exists in your environment)
gpt_model = GPT4All("mistral-7b-instruct-v0.1.Q4_0.gguf")

# ============================================
# Define GPT4All Prompt for Meeting Analysis
input_text = f"""
You are an expert meeting assistant. Analyze the following meeting transcript and extract the key discussion points, decisions made, and action items.

### Transcript ###
{final_transcript}

### Expected Output ###
Give the summarized meeting report first, followed by:

- *Key Discussion Points*:
- *Decisions Made*:
- *Action Items*:
"""

# ============================================
# Generate Structured Output using GPT4All
with gpt_model.chat_session():
    gpt_output = gpt_model.generate(input_text, max_tokens=800, temp=0.7)

# ============================================
# Parse GPT Output into Lists
summary_report = []
discussion_points = []
decisions_made = []
action_items = []
def parse_gpt4all_summary(gpt_output):
    """
    Parse the GPT4All output into structured lists (summary report, discussion points, decisions made, action items)
    """

    sections = re.split(r"- \\(.?)\\*:", gpt_output)


    current_section = None

    for i, section in enumerate(sections):
        section = section.strip()

        if i == 0:
            summary_report = [line.strip("- ") for line in section.split("\n") if line.strip()]
        elif section.lower().startswith("key discussion points"):
            current_section = discussion_points
        elif section.lower().startswith("decisions made"):
            current_section = decisions_made
        elif section.lower().startswith("action items"):
            current_section = action_items
        elif current_section is not None:
            items = [line.strip("- ") for line in section.split("\n") if line.strip()]
            current_section.extend(items)

        # Initialize Trello client
        client = TrelloClient(
            api_key='Add_api_key',
            token='Add_your_api_token'
        )


        # Create a new board
        board_name = "kkrn10000"
        board = client.add_board(board_name)
        print(f"Created board: {board.name} (ID: {board.id})")

        # Add lists to the board
        action_items_list = board.add_list("Action Items")
        print(f"Created list: {action_items_list.name} (ID: {action_items_list.id})")

        discussion_points_list = board.add_list("Discussion Points")
        print(f"Created list: {discussion_points_list.name} (ID: {discussion_points_list.id})")

        summary_report_list = board.add_list("Summary Report")
        print(f"Created list: {summary_report_list.name} (ID: {summary_report_list.id})")

        decisions_made_list = board.add_list("Decisions Made")
        print(f"Created list: {decisions_made_list.name} (ID: {decisions_made_list.id})")

        # Add cards to the Action Items list
        for item in action_items:
            card = action_items_list.add_card(item)
            print(f"Added card to Action Items: {card.name}")

        # Add cards to the Discussion Points list
        for point in discussion_points:
            card = discussion_points_list.add_card(point)
            print(f"Added card to Discussion Points: {card.name}")

        # Add cards to the Summary Report list
        for report in summary_report:
            card = summary_report_list.add_card(report)
            print(f"Added card to Summary Report: {card.name}")

        # Add cards to the Decisions Made list
        for decision in decisions_made:
            card = decisions_made_list.add_card(decision)
            print(f"Added card to Decisions Made: {card.name}")

        print(f"Board '{board_name}' created successfully with lists and cards!")


    return {
        "summary_report": summary_report,
        "discussion_points": discussion_points,
        "decisions_made": decisions_made,
        "action_items": action_items
    }
parsed_data = parse_gpt4all_summary(gpt_output)


