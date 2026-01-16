# 🌐 Universal Translator (Colab-Ready) — NLLB + Helsinki + mBART

A **production-style multilingual translation system** designed for **Google Colab / Jupyter Notebooks**, powered by **latest Hugging Face Transformers**.  
It supports **200+ languages** using **NLLB-200**, with **fast pair-based translation** using **Helsinki-NLP OPUS**, plus **optional mBART-50** support.

This project includes:
- ✅ A clean **Python API**
- ✅ **Interactive Jupyter Widget UI**
- ✅ **Batch translation**
- ✅ **Caching** for faster repeated translations
- ✅ GPU acceleration (when available in Colab)

---

## ✨ Key Features

### 🔥 Modern Translation Engines
- **NLLB-200 (facebook/nllb-200-distilled-600M)**  
  Best for universal multilingual translation with **200+ languages**.
- **Helsinki-NLP OPUS models**  
  Very fast for common language pairs like **English ↔ Spanish/French/German/Russian/Arabic**.
- **mBART-50 (facebook/mbart-large-50-many-to-many-mmt)** *(optional fallback)*  
  Useful for many-to-many multilingual translation scenarios.

### ⚡ Fast & Efficient
- **Auto engine selection** (Helsinki when available → else NLLB)
- **GPU support in Colab** for faster inference
- **Translation caching** to avoid repeated compute

### 🎮 Interactive Notebook UI
- Jupyter/Colab widget interface with:
  - Input textarea
  - Source/target language dropdowns
  - Engine selection dropdown
  - Output panel with details
  - Cache stats + cache clear

### 📦 Batch Translation
Translate lists of texts efficiently:
- Use for datasets, CSV columns, chatbot logs, etc.

### 🧠 Auto Language Detection
Uses `langdetect` to automatically detect source language when set to `auto`.

---

## 📌 Benefits (Why use this project?)

- **No unstable dependencies** like `googletrans` (avoids `httpx` conflicts)
- Uses **official and maintained models** from Hugging Face
- Works reliably in **Google Colab**
- Clean architecture: caching + engine routing + batch support
- Easily extendable into:
  - Streamlit Web App
  - Django / FastAPI API service
  - Mobile app translation backend

---

## 🧱 Tech Stack

- **Python 3.10+**
- **transformers**
- **torch**
- **accelerate**
- **sentencepiece**
- **sacremoses**
- **langdetect**
- **langcodes**
- **ipywidgets**
- (Optional) **deep-translator**

---

## ✅ Supported Languages

This repo includes a curated set of popular languages in the UI mapping:

- English 🇬🇧
- Spanish 🇪🇸
- French 🇫🇷
- German 🇩🇪
- Chinese 🇨🇳
- Arabic 🇸🇦
- Hindi 🇮🇳
- Russian 🇷🇺
- Japanese 🇯🇵
- Portuguese 🇵🇹
- Italian 🇮🇹
- Dutch 🇳🇱
- Korean 🇰🇷
- Turkish 🇹🇷
- Vietnamese 🇻🇳
- Thai 🇹🇭
- Swahili 🇰🇪
- Urdu 🇵🇰
- Persian 🇮🇷
- Bengali 🇧🇩

> **Note:** NLLB supports **200+ languages**.  
> You can extend the `LANGUAGE_MAPPING` anytime.

---

## ⚙️ Installation (Google Colab)

Run this in a Colab cell:

```bash
pip -q install -U transformers accelerate sentencepiece sacremoses langdetect langcodes language_data ipywidgets
```

---

## 🚀 Quick Start

### 1️⃣ Create Translator

```python
translator = LanguageTranslator()
translator.display_device_info()
```

### 2️⃣ Translate Text

```python
translator.translate(
    "Hello, how are you?",
    target_language="french",
    source_language="auto",
    engine="auto"
)
```

Example output:

```json
{
  "original_text": "Hello, how are you?",
  "translated_text": "Bonjour, comment allez-vous ?",
  "engine_used": "helsinki",
  "cached": false
}
```

---

## 🔁 Batch Translation

```python
texts = [
    "Hello world",
    "Good morning",
    "Thank you"
]

results = translator.batch_translate(texts, target_language="german")
for r in results:
    print(r["original_text"], "→", r["translated_text"])
```

---

## 🎛️ Engines / Modes

The translator supports the following engine modes:

| Engine        | Value      | Best For                                   |
| ------------- | ---------- | ------------------------------------------ |
| Auto Select   | `auto`     | Smart selection (Helsinki → NLLB fallback) |
| NLLB-200      | `nllb`     | 200+ language universal translation        |
| Helsinki OPUS | `helsinki` | Fast pair-based translation                |
| mBART-50      | `mbart`    | Many-to-many multilingual fallback         |

---

## 🧠 Language Detection

When `source_language="auto"`:

```python
translator.detect_language("Bonjour tout le monde")
```

Example:

```
('fr', 'French')
```

---

## 💾 Caching

Repeated translations are served instantly using in-memory cache.

### Cache Stats

```python
translator.cache.get_stats()
```

Example:

```json
{
  "hits": 4,
  "misses": 7,
  "size": 7,
  "efficiency": 36.4
}
```

### Clear Cache

```python
translator.cache.clear()
```

---

## 🖥️ Interactive Widget UI (Colab)

Run:

```python
create_widget()
```

You’ll get:

* Input text area
* Source + target language selectors
* Engine selector
* Translate button
* Output box with engine + timing info
* Cache stats

> ⚠️ If widgets don’t appear in Colab:

* Runtime → Restart runtime
* Ensure `ipywidgets` is installed
* Run the install cell again

---

## 🔧 Configuration

### Extend Language Support

To add a new language to UI dropdown, add it in:

```python
LANGUAGE_MAPPING = {
   ...
   "tamil": {"code": "tam_Taml", "name": "Tamil", "emoji": "🇮🇳"},
}
```

> Make sure the `code` is a valid **NLLB language code**.

### Add More Helsinki Models

```python
HELSINKI_MODELS["en-it"] = "Helsinki-NLP/opus-mt-en-it"
HELSINKI_MODELS["it-en"] = "Helsinki-NLP/opus-mt-it-en"
```

---

## 🧪 Performance Tips

### Enable GPU in Colab

Go to:
**Runtime → Change runtime type → GPU**

Then rerun the notebook.

### Reduce Memory Usage

Use NLLB distilled model (already used):

* `facebook/nllb-200-distilled-600M`

### Faster Translation for Common Pairs

Use engine:

```python
engine="helsinki"
```

---

## 🛡️ Known Limitations

* This is **inference-only** (no fine-tuning included)
* Some rare languages may produce lower-quality translations depending on model coverage
* Very long text should be chunked (recommended for paragraphs/pages)

---

## 🗺️ Roadmap / Future Improvements

* [ ] Add long-text chunking with sentence split
* [ ] Add PDF/DOCX translation
* [ ] Add FastAPI REST API (mobile app support)
* [ ] Add Streamlit web UI + deployment
* [ ] Add translation memory (persistent DB cache)

---

## 📄 License

This project is intended for educational and internal usage.
Model licenses are governed by Hugging Face and their respective publishers:

* Meta/Facebook NLLB
* Helsinki-NLP OPUS
* mBART

---

## 👨‍💻 Author / Maintainer

Built for **Colab-friendly multilingual translation** using modern AI models.
If you want the **API version for Django/FastAPI**, ask and it can be structured as a deployable service.

---
