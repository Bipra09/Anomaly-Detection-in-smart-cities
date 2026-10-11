# Anomaly Detection in Smart Cities

AI-powered video surveillance analysis for urban environments. Upload a video and a vision-language model (running locally through Ollama) classifies what's happening, rates its confidence, and recommends an action.

## Features
- Detects and classifies unusual events in city video footage
- Vision LLM analysis with structured JSON output (Pydantic schema)
- Analyzes a sequence of frames together, not single images
- Confidence level (Low / Medium / High) and recommended action per analysis
- Results overlaid on the video frames and shown as an animation
- Logging of every run to `logs/`
- Runs in Google Colab

## Detected Events
| Category | Examples |
|---|---|
| Normal | regular traffic, pedestrians |
| Accident | vehicle collisions, structural collapse |
| Fire | visible flames, dense persistent smoke |
| Violence | punching, kicking, weapon use |
| Theft / Vandalism | snatching, break-ins, property damage |
| Public Disturbance | riots, panic, aggressive crowds |
| Medical Emergency | sudden collapse, seizure |
| Other | unusual safety risk that fits no category |

## How It Works
1. Install Ollama and start the service in the background
2. Pull the vision model (`gemma3:12B`)
3. Upload a video through the notebook widget
4. Sample every 5th frame
5. Send the sampled frames to the model in one request, with a detailed system prompt
6. The model returns JSON: classification, confidence, description, recommended action
7. The result is drawn on the frames and displayed as an animation with a summary

## Tech Stack
- Python
- Ollama (`gemma3:12B`)
- OpenCV
- Pydantic
- Google Colab, ipywidgets, Matplotlib

## Project Structure
├── Main.ipynb # setup, model pull, inferencer class, upload UI
└── README.md

## Setup
1. Open `Main.ipynb` in Google Colab with a **GPU runtime**
2. Run the cells in order:
   - Install `pciutils` and Ollama
   - Start `ollama serve`
   - `ollama pull gemma3:12B`
   - Install `ollama` and `opencv-python`
3. Run the class definition cell, then `main_colab_batch()`
4. Click **Upload & Process Video** and select a short clip

## Configuration
In `main_colab_batch()`:
```python
MODEL_NAME = "gemma3:12B"   # any Ollama vision model, e.g. llava:13b
SAMPLE_RATE = 5             # use every Nth frame
```

## Example Output
Status: VIOLENCE
Confidence: Medium
Details: Two individuals exchange punches and grapple on the ground...
Action: Dispatch security personnel to investigate.


## Limitations
- All sampled frames go in a single request, so use short clips
- Output depends on the model and can be wrong. Treat results as alerts for human review
- Not a real-time stream processor: the video is read first, then analyzed

## Future Improvements
- Live camera/RTSP streams
- Sliding-window analysis for long videos
- Alert notifications (SMS / email)
- Dashboard for multiple cameras

## License
MIT
