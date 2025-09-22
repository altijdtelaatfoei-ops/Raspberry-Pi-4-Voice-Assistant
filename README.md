# Raspberry-Pi-4-Voice-Assistant

Een lichte, privacygerichte spraakassistent die volledig offline draait op je Raspberry Pi 4.
Gebouwd met:

* **OpenAI Whisper** voor spraakherkenning
* **Ollama** voor intelligente antwoorden
* **KittenTTS** voor natuurlijk klinkende spraaksynthese

Alles wordt lokaal op je apparaat verwerkt, zonder cloudverbinding.

---

## Functies

* 100% lokaal – geen internet nodig na installatie
* Realtime spraakherkenning met Whisper Tiny (39MB)
* Geoptimaliseerd voor Raspberry Pi 4
* Lokale AI-antwoorden met Ollama en gemma3:270m model
* Neurale tekst-naar-spraak met KittenTTS (25MB)
* Privacy gegarandeerd – gesprekken blijven lokaal
* Weinig systeembronnen nodig – werkt soepel op Pi 4 met 4GB RAM
* Eenvoudige CLI-interface (command-line bediening)
* Gespreksgeheugen – onthoudt context tussen vragen
* Meerdere TTS-stemopties (8 verschillende stemmen)

---

## Snel starten

### Benodigdheden

* Raspberry Pi 4 (minimaal 4GB RAM, 8GB aanbevolen)
* Python 3.8+
* Microfoon en luidsprekers/koptelefoon
* Ollama geïnstalleerd en draaiend
* MicroSD-kaart (32GB+ aanbevolen)

### Installatie

1. Repository klonen:

   ```bash
   git clone https://github.com/dwain-barnes/ondevice-pi-voice-assistant.git
   cd ondevice-pi-voice-assistant
   ```

2. Virtuele omgeving maken:

   ```bash
   python3 -m venv ondevice-voice-env
   source ondevice-voice-env/bin/activate
   ```

3. Afhankelijkheden installeren:

   ```bash
   pip install -r requirements.txt
   pip install https://github.com/KittenML/KittenTTS/releases/download/0.1/kittentts-0.1.0-py3-none-any.whl
   sudo apt-get install portaudio19-dev
   curl -fsSL https://ollama.com/install.sh | sh
   ```

4. Ollama instellen:

   ```bash
   curl -fsSL https://ollama.ai/install.sh | sh
   ollama run gemma3:270m
   ```

5. Start de assistent:

   ```bash
   python pi-voice-assistant.py
   ```

Bij de eerste keer gebruik wordt automatisch het gemma3:270m model (\~270MB) gedownload. Daarna draait alles volledig lokaal.

---

## Gebruik

* Druk **ENTER** om opname te starten
* Druk nogmaals **ENTER** om opname te stoppen en een antwoord te krijgen
* Beschikbare commando’s:

  * `voice` – wijzig TTS-stem (8 opties)
  * `clear` – wis gespreksgeschiedenis
  * `quit` – stop de assistent

---

## Vereisten

### Software

* `openai-whisper` – spraakherkenning
* `torch` – neural network framework
* `ollama` – lokale AI-client
* `kittentts` – tekst-naar-spraak
* `pyaudio` – audio-invoer/uitvoer
* `soundfile`, `numpy` – audio en berekeningen

### Hardware

* Minimum: Raspberry Pi 4, 4GB RAM, 32GB microSD, USB-microfoon
* Aanbevolen: Raspberry Pi 4 (8GB RAM), snelle SD-kaart (Class 10/U3), hoogwaardige microfoon

---

## Configuratie

Pas instellingen aan in `pi-voice-assistant.py`:

```python
self.ollama_model = "gemma3:270m"  # AI-model
self.whisper_model = "tiny"        # opties: tiny, base, small, medium, large
self.tts_voice = "expr-voice-2-f"  # standaardstem (8 opties)
```

---

## Beschikbare TTS-stemmen

* Mannelijk: expr-voice-2-m, expr-voice-3-m, expr-voice-4-m, expr-voice-5-m
* Vrouwelijk: expr-voice-2-f, expr-voice-3-f, expr-voice-4-f, expr-voice-5-f

---

## Architectuur

```
Microfoon (Audio In) → Whisper Tiny (spraak→tekst) → Ollama (AI)
                                  ↓
Speakers (Audio Out) ← KittenTTS (tekst→spraak)
```

---

## Prestaties

* Whisper Tiny: \~39MB
* KittenTTS: \~25MB
* Gemma3:270m: \~270MB
* Totaal: \~334MB

Geheugenverbruik op Raspberry Pi 4 (4GB RAM):

* Idle: \~200MB
* Tijdens verwerking: \~800MB–1.2GB
* Reactietijd: 4–9 seconden

---

## Problemen oplossen

**Audio werkt niet**
Controleer beschikbare audio-apparaten:

```bash
python -c "import pyaudio; p=pyaudio.PyAudio(); [print(f'{i}: {p.get_device_info_by_index(i)[\"name\"]}') for i in range(p.get_device_count())]"
```

**Ollama reageert niet**

```bash
ollama list
ollama serve
```

**KittenTTS installatieproblemen**

```bash
pip install --upgrade pip
pip install https://github.com/KittenML/KittenTTS/releases/download/0.1/kittentts-0.1.0-py3-none-any.whl --force-reinstall
```

---
