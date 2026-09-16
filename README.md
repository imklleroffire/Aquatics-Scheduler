# Aquatics Scheduler

Streamlit app that builds swim-lesson schedules from instructor availability and class templates for the **YGS Sammamish Aquatics** swim department. It replaces a slow manual spreadsheet workflow with template upload, instructor management, conflict-aware assignment, and Excel export.

## Why it exists

Swim schedules were assembled by hand from availabilities and class lists. This tool cuts that effort dramatically by generating a formatted schedule from the same inputs.

## Tech stack

- **Python 3.8+** / **Streamlit** — UI and app shell
- **Pandas** / **OpenPyXL** — spreadsheet parsing and export
- **Plotly** — charts
- **Firebase** (Auth + Admin SDK via `firebase-admin` / Pyrebase) — sign-in and data
- **OpenAI** (optional) — assistant features when `OPENAI_API_KEY` is set

## Quick start

```bash
git clone https://github.com/imklleroffire/Aquatics-Scheduler.git
cd Aquatics-Scheduler
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

Open http://localhost:8501

### Firebase (required for full auth)

Local Admin SDK: place `serviceAccountKey.json` in the project root (gitignored).  
Streamlit Cloud: put the same service-account fields in **Secrets** (see `STREAMLIT_DEPLOYMENT.md` — use placeholders only; never commit real keys).

Sign in with a Firebase Auth email/password account for your project. Optional AI features need:

```bash
export OPENAI_API_KEY=your_key_here
```

## Typical workflow

1. Sign in (supervisor/admin flows as configured in Firebase).
2. Upload or select a class template (Excel).
3. Enter instructors and availability.
4. Generate the schedule and download the Excel output.

## Project layout

```
Aquatics-Scheduler/
├── app.py                 # Streamlit entry point
├── auth.py                # Sign-in / sign-up UI
├── scheduler.py           # Assignment logic
├── enrollment_parser.py   # Enrollment / template parsing helpers
├── template_parser.py     # Template helpers
├── instructor_manager.py  # Instructor data helpers
├── firebase_config.py     # Firebase client/admin wiring
├── ai_assistant.py        # Optional OpenAI assistant
├── ui_components.py       # Shared UI pieces
├── config.py              # App constants
├── requirements.txt
├── assets/                # CSS + Excel templates
└── data/                  # Sample instructor JSON
```

Extra markdown guides in the repo (`STREAMLIT_DEPLOYMENT.md`, Firebase notes, etc.) are operational docs, not required to understand the core app.

## Development checks

```bash
pip install -r requirements.txt
python test_app_fixes.py
```

## License

MIT — see [LICENSE](LICENSE).

## Security

- Do **not** commit `serviceAccountKey.json`, `.env`, or Streamlit secrets.
- If a service-account private key was ever committed, rotate it in Google Cloud / Firebase Console.
