# Free Youtube summarizer extension

Created by Aldrin Gustavo Stori
Inspired by
<br>
<https://github.com/ayaansh-roy/llm_chrome_ext_summerizer>
<br>
<https://github.com/xingyuqiu2/youtube-translator-summarizer-extension>

## Motivation

Watching YouTube videos can often be time-consuming, with crucial information typically scattered throughout the content, sometimes comprising only about 20% of the video. Tools like Sider are incredibly effective in saving time by providing concise summaries that allow you to quickly access the main points. However, free summaries are often limited, and premium options require payment. To help you save both time and money, I have developed this extension, you can use the LLM of your preference.

## Simple explanation how the software works
1. User opens YouTube and clicks on 'Summary' button.
2. A request is sent to the Flask server.
3. Flask server sends a request to the local LLM API.
4. Local LLM API processes the request and sends a summary back to the Flask server.
5. Flask server sends the summary back to the Chrome extension.
6. The Chrome extension displays the summary in the view box on YouTube.

## Installation

1. Install LLM model (phi3 is the default, but you can use any LLM of your choice.)

```
ollama run phi3
```

2. Clone the repository

```
git clone https://github.com/A-GustavoStori/free-yt-summarizer
```

3. Cd to the project folder and run

```
pip install -r requirements.txt
```

4. Open a terminal or command prompt, navigate to the directory containing llm_service.py. and app.py
Run

```
python llm_service.py
python app.py
```

5. Install the extension
Go into chrome extensions, enable developer mode and click on load unpacked extension, select the folder containing the extension.


## Documentation

1. **User Opens YouTube:**
    
    - The user navigates to a YouTube video page (e.g., `https://www.youtube.com/watch?v=videoId`).

2. **Content Script Injection:**
    
    - The Chrome extension's content script (`contentScript.js`) is automatically injected into the YouTube video page.

3. **UI Creation:**
    
    - The `createUI` function is called, which creates a user interface (UI) for the extension.
    - This UI consists of a container (`extensionContainer`), a header with an icon and title, and a summary button (`summaryButton`).
    - The UI is appended to the body of the YouTube page.

4. **User Clicks the Summary Button:**
    
    - The user clicks the "Get Summary" button (`summaryButton`).

5. **Click the Transcription Button:**
    
    - The `handleSummaryButtonClick` function is triggered by the button click.
    - This function calls `clickTranscriptionButton`, which simulates a click on the YouTube transcription button to load the video transcript.
    - The transcription button is identified by the CSS class `.yt-spec-touch-feedback-shape__fill`.

6. **Wait for Transcript to Load:**
    
    - A delay (set to 2000 milliseconds) is introduced using `setTimeout` to allow the transcript to fully load.

7. **Extract Transcript Text:**
    
    - After the delay, the `getTranscriptText` function is called.
    - This function selects all elements with the class `segment-text` within `ytd-transcript-segment-renderer` to extract the transcript text.
    - The text content of each segment is concatenated into a single string (`transcriptText`).

8. **Send Request to Flask Server:**
    
    - The `fetchSummary` function is called with the extracted transcript text as the argument.
    - This function prepares a payload containing the transcript text and sends a POST request to the Flask server running at `http://127.0.0.1:5000/summarize`.

9. **Flask Server Handles Request:**
    
    - The Flask server (`llm_service.py`) receives the POST request at the `/summarize` endpoint.
    - The server extracts the `text` from the request payload and calls the `generate_text` function.

10. **Generate Summary Using LLM:**
    
    - The `generate_text` function sends a POST request to the `phi3 ollama` LLM running on `http://localhost:11434/api/generate` with the provided prompt.
    - The LLM processes the prompt and generates a summary.
    - The generated summary is returned to the Flask server.

11. **Respond with Summary:**
    
    - The Flask server returns the summary as a JSON response to the content script.

12. **Display Summary:**
    
    - The content script receives the response and extracts the summary text.
    - The `loadSummary` function is called with the summary text.
    - This function updates the summary container (`summaryContainer`) in the UI with the generated summary.


