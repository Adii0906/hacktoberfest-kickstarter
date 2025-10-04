
# Hacktoberfest Mentor 2025

AI-powered, Streamlit-based mentor to help contributors find Hacktoberfest-friendly issues and build a proper profile from resumes.

This repository contains a small web app and services that: (1) let users upload a resume and automatically extract skills/experience using AI, and (2) discover and recommend GitHub issues that match a user's skills and interests.

## Table of contents

- About
- Features
- Tech stack
- Quick start (Windows / PowerShell)
- Configuration
- Running the app
- Important files
- Resume upload & AI analysis
- Testing
- Contributing
- Security & privacy
- Next steps / roadmap

## About

The goal of this project is to lower the barrier to open-source contribution during Hacktoberfest by:

- Letting users build a profile quickly by uploading a resume.
- Using local/remote AI models to analyze issues and rank them for the user's skill level.
- Guiding new contributors through forking, cloning, and submitting pull requests.

This is implemented as a Streamlit app with modular services under `services/` for AI and resume processing.

## Features

- Streamlit web UI with multi-page navigation
- Resume upload (PDF / DOCX) and AI-powered profile extraction
- Issue discovery and AI recommendations (Ollama / fallback scoring)
- Session-based GitHub token support for higher API limits
- Clear, beginner-friendly contribution guide integrated in the UI

## Tech stack

- Python 3.8+ (project files reference 3.8+ and have been developed with Streamlit)
- Streamlit for the frontend (`streamlit_app.py` and `pages/`)
- AI integrations:
	- Google Generative AI (Gemini) client used by `services/resume_service.py`
	- Ollama client used by `services/ai_service.py` (optional)
- PDF/DOCX parsing: `pdfplumber`, `PyPDF2`, `python-docx`

Required Python packages are declared in `requirements.txt`.

## Quick start (Windows / PowerShell)

1. Clone the repository (if you haven't already):

```powershell
git clone https://github.com/Adii0906/hacktoberfest-kickstarter.git
cd hacktoberfest-kickstarter
```

2. Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

3. Install dependencies:

```powershell
pip install -r requirements.txt
```

4. (Optional) Set environment variables for API keys. It's recommended to export these in your session rather than hard-coding them:

```powershell
$env:GITHUB_TOKEN = "ghp_xxx_your_token_here"
$env:GEMINI_API_KEY = "your_gemini_api_key_here"
```

Note: The app will work in demo mode without API keys, but setting these improves recommendation quality and API limits.

## Configuration

- GitHub token: set `GITHUB_TOKEN` in environment for higher GitHub API limits (recommended).
- Gemini AI: provide `GEMINI_API_KEY` for the resume analysis service (`services/resume_service.py`) or use the mock service for local testing.
- Ollama: install and run Ollama locally if you want the `services/ai_service.py` to call a local model. See Ollama docs for installation and running a model.

Important: Do not commit production API keys to the repository. If you find any hard-coded keys in source, rotate them and use environment variables.

## Running the app (Streamlit)

Start the Streamlit app from the repository root:

```powershell
streamlit run streamlit_app.py
```

Open the URL printed by Streamlit in your browser (usually http://localhost:8501).

Pages included:
- `01_🏠_Profile_Setup.py` — profile creation and resume upload
- `02_🔍_Find_Issues.py` — issue discovery and recommendations
- `03_📖_Contribution_Guide.py` — step-by-step guide to submit PRs

## Important files

- `streamlit_app.py` — main Streamlit app and navigation
- `services/resume_service.py` — resume parsing + Gemini-based extraction
- `services/ai_service.py` — issue analysis using Ollama (plus fallback scoring)
- `services/mock_resume_service.py` — mock/demo resume processor for local testing
- `pages/` — Streamlit multi-page UI components
- `requirements.txt` — Python dependencies

## Resume upload & AI analysis

The app supports uploading resumes (PDF, DOCX). Uploaded files are processed in-memory and analyzed to extract:

- experienceLevel: beginner | intermediate | advanced
- programmingSkills: list matched against a predefined SKILLS_LIST
- areasOfInterest: list matched against a predefined INTERESTS_LIST

By default the repository includes a production-ready `services/resume_service.py` that calls Google Generative AI (Gemini) and a `mock_resume_service.py` used for local/demo mode. Switch between them in `pages/01_🏠_Profile_Setup.py` by importing the desired service.

Security & privacy: resume text is intended to be processed in-memory and not stored persistently. Review and sanitize any configuration before deploying to a shared environment.

## Testing

There are a couple of small tests in the repo for the mock resume flow. Run them with pytest (install pytest if needed):

```powershell
pip install pytest
pytest -q
```

Or run a single test file:

```powershell
python test_mock_resume_service.py
```

## Contributing

Contributions are welcome — especially around these areas:

- improving AI prompts and parsing robustness
- expanding supported resume formats and edge-case parsing
- adding CI, unit tests, and linting
- secure configuration (removing any hard-coded keys)

Suggested workflow:

1. Fork the repository
2. Create a feature branch
3. Add tests for new behavior
4. Open a PR with a clear description and linked issues

Please mark Hacktoberfest-friendly issues with the `hacktoberfest` label where appropriate.

## Security & privacy notes

- Never commit API keys or tokens. Use environment variables or a secrets manager.
- Uploaded resumes should be processed in-memory; if you add persistence, explicitly document retention and access controls.
- There may be example keys or placeholders in the repository; rotate and remove these before publishing or deploying.

## Next steps / roadmap

- Add CI (GitHub Actions) for linting and tests
- Add an explicit `LICENSE` file (MIT recommended) and a `CODE_OF_CONDUCT.md`
- Add confidence scoring to AI outputs and show them in the UI
- Support batch resume uploads and additional file types (TXT/DOC)

## Contact

If you want to collaborate, open an issue or a PR. For quick questions you can reach the repository owner at their GitHub profile: https://github.com/Adii0906

---

Happy hacking and good luck with Hacktoberfest 2025! 🎃
