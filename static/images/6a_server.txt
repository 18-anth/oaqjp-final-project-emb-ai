"""Flask web server for Emotion Detection application."""

from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask(__name__)


@app.route('/')
def render_index_page():
    """Render the main index page."""
    return render_template('index.html')


@app.route('/emotionDetector')
def emotion_detector_route():
    """
    Analyze emotions in the provided text and return a formatted response.

    Query Parameters:
        textToAnalyze (str): The text to analyze for emotions.

    Returns:
        str: A formatted string with emotion scores and the dominant emotion,
             or an error message if the input is blank or invalid.
    """
    text_to_analyse = request.args.get('textToAnalyze', '')

    result = emotion_detector(text_to_analyse)

    if result['dominant_emotion'] is None:
        return "Invalid text! Please try again!"

    response = (
        f"For the given statement, the system response is "
        f"'anger': {result['anger']}, "
        f"'disgust': {result['disgust']}, "
        f"'fear': {result['fear']}, "
        f"'joy': {result['joy']} and "
        f"'sadness': {result['sadness']}. "
        f"The dominant emotion is <b>{result['dominant_emotion']}</b>."
    )
    return response


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
