# Emotion Analyzer

A natural language processing project for analyzing Turkish song lyrics to predict musical genre and emotional content using pre-trained transformer models.

---

## Overview

Emotion Analyzer processes Turkish song lyrics through two parallel pipelines: a keyword-based genre classifier and a pre-trained sentiment model. The tool outputs genre prediction, emotional tone, and a short summary of the lyrics — each accompanied by a confidence score.

The project was developed as an NLP study, exploring how pre-trained Hugging Face models can be applied to Turkish text without additional fine-tuning.

---

## Features

- Genre classification based on domain-specific keyword lexicons (arabesk, rock, halk muzigi, rap, sanat muzigi, and more)
- Emotion detection using a pre-trained Turkish sentiment model (positive/negative with mapped emotional labels)
- Automatic summarization of lyrics using a pre-trained seq2seq model
- Confidence scoring displayed with a visual progress bar in the terminal
- Three usage modes: interactive, direct argument, and CSV file input

---

## Project Structure

```
Emotion-Analyzer/
├── 1_kurulum_test.py         # Environment and dependency check
├── 2_model_indir_kaydet.py   # Downloads and saves models locally
├── 3_veri_analiz.py          # Data exploration and analysis
├── 4_sarki_analiz.py         # Main analysis script
├── third.py                  # Lexicon-only weighted analysis (no pretrained model)
├── turkce_sarki_lexicon.csv  # Turkish song lyric keyword lexicon
├── ornekSarki1.csv           # Sample input files
├── ornek2.csv
├── ornek3.csv
└── models/                   # Downloaded via Google Drive (see below)
    ├── turkish_sentiment/
    └── turkish_summary/
```

---

## Requirements

- Python 3.9 or higher
- PyTorch
- Hugging Face Transformers

---

## Setup

**1. Clone the repository**

```bash
git clone https://github.com/ogulcantekines/Emotion-Analyzer.git
cd Emotion-Analyzer
```

**2. Create and activate a virtual environment**

```bash
python -m venv venv

# macOS / Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

**3. Install dependencies**

```bash
pip install torch transformers
```

**4. Download the pre-trained models**

The model files are hosted on Google Drive due to their size. Download the `models/` folder and place it in the project root.

[Download Models from Google Drive](https://drive.google.com/drive/folders/1Gu_lL2LFfH8lT83EABS40ZMru0Xrhmcr?usp=sharing)

Your directory should look like:

```
Emotion-Analyzer/
└── models/
    ├── turkish_sentiment/
    └── turkish_summary/
```

---

## Usage

### Interactive mode

```bash
python 4_sarki_analiz.py
```

You will be prompted to enter an artist name and song lyrics line by line. Type `---` on its own line to submit.

### Direct lyric input

```bash
python 4_sarki_analiz.py --sarki "Şarkı sözleri buraya..." --sanatci "Duman"
```

### CSV file input

The CSV file should have a `singer;lyrics` format with one song per row.

```bash
python 4_sarki_analiz.py --dosya ornekSarki1.csv
```

---

## Sample Output

```
-------------------------------------------------------
  Artist   : Duman
  Lyrics   : [Nakarat] Elleri havada...
-------------------------------------------------------
  Genre    : rock
  95%  ████████████████████░░░░  Very confident

  Emotion  : deep sorrow / pain
  97%  ███████████████████████░  Very confident

  Summary  : ...
-------------------------------------------------------
```

Confidence levels:

| Score     | Label              |
|-----------|--------------------|
| 90% +     | Very confident     |
| 75 - 89%  | Confident          |
| 60 - 74%  | Moderate           |
| Below 60% | Low — use caution  |

---

## Alternative Analysis Mode

`third.py` runs a purely lexicon-based analysis without loading any pre-trained model. Genre and emotion predictions are derived entirely from weighted keyword frequencies in `turkce_sarki_lexicon.csv`.

```bash
python third.py --dosya sozler1.csv
```

---

## Notes

- This project performs **inference** using pre-trained models. No model training takes place locally.
- The sentiment model was originally trained on Turkish text and is applied as-is to song lyrics.
- Genre classification accuracy depends on the richness of the keyword lexicon and may default to "pop" when no keywords match.
- MPS (Apple Silicon) and CUDA are both supported; the device is selected automatically.

---

## License

This project is for educational and research purposes.