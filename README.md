# Multi-Language Evaluation

Evaluate AI/Copilot responses across multiple languages using machine translation and simple similarity comparisons, with inputs/outputs in Excel.

## What this project does
- **Translates questions** to one or more target languages (Google Translate via `googletrans`).
- **Calls an AI API** with the translated question to get an answer (OAuth client-credentials flow).
- **Optionally translates answers back** to English for normalization.
- **Writes results to Excel** for review and further analysis.
- **Compares answers** using a basic string similarity metric (see `CompareResults.py`).

## Repository structure
- `MultiLang_Evaluation.py`: Main script to translate questions, call the AI API, and export results to Excel.
- `CompareResults.py`: Utility to compare a ground-truth answer against other answers using similarity percentage.
- `questions.xlsx`: Input Excel file. Must contain a column named `Question`.
- Other Excel files (e.g., `translated_answers.xlsx`, `ReportChart.xlsx`) are optional/output artifacts.

## Prerequisites
- **Python**: 3.9 or newer recommended
- **Network access** for translation and API calls
- **Excel** reader/writer support via `openpyxl`
- **AI API access**: The script is currently configured for BoldDesk AI endpoints. You must have:
  - A reachable token endpoint and resource endpoint
  - Valid `client_id`, `client_secret`, and any required IDs (e.g., `ticketId`)

## Installation
1. (Recommended) Create and activate a virtual environment.
   - Windows (PowerShell):
     ```powershell
     python -m venv .venv
     .venv\Scripts\Activate.ps1
     ```
2. Install dependencies:
   ```bash
   pip install pandas requests aiohttp nltk openpyxl googletrans==4.0.0rc1
   ```

   Notes:
   - `googletrans` provides `Translator` used for translations.
   - `nltk` is included as `sentence_bleu` is referenced; no extra corpora are required for this function.

## Configuration
Open `MultiLang_Evaluation.py` and review/update:
- **OAuth/token endpoint** (`base_app_link`) and **resource endpoint** (`url`).
- **Credentials**: `client_id`, `client_secret`, and `scope` (if applicable).
- **Request payload** fields like `ticketId`.
- **Target languages** list (`languages` variable). Defaults to `['es', 'no']`.
- **Input/Output** Excel paths: `input_file` (default `questions.xlsx`) and `output_file` (default `GoogleTransNormal.xlsx`).

Also ensure the following import exists near the top if it is missing:
```python
from googletrans import Translator
```

## Input file format
`questions.xlsx` must include a column named `Question` containing the text prompts to evaluate.

## Quick start
1. Place your prompts in `questions.xlsx` under the `Question` column.
2. Configure endpoints, credentials, languages, and file paths in `MultiLang_Evaluation.py`.
3. Run the main script:
   ```bash
   python MultiLang_Evaluation.py
   ```
4. Check the output Excel file (default `GoogleTransNormal.xlsx`) for the translated questions and generated answers.

## Comparing answers (optional)
Use `CompareResults.py` to compute a simple similarity percentage between a ground-truth answer and other answers.

1. Create `compare.xlsx` with:
   - Row 1, Col 1: ground-truth answer
   - Subsequent rows in Col 1: other answers to compare
2. Run:
   ```bash
   python CompareResults.py
   ```
3. The script prints similarity scores (%) to the console.

## Troubleshooting
- **`NameError: Translator is not defined`**
  - Install `googletrans==4.0.0rc1` and add `from googletrans import Translator` in `MultiLang_Evaluation.py`.
- **401/403/400 errors from API**
  - Verify token endpoint, resource endpoint, credentials, and that your AI API server is reachable.
- **Excel read/write issues**
  - Ensure `openpyxl` is installed and the file is not locked by another program.
- **Language codes**
  - Use valid ISO language codes supported by Google Translate (e.g., `es`, `de`, `fr`, `ja`, `zh-CN`).

## Notes
- The BLEU score function is present but not currently invoked in the main flow. You can integrate it as needed for quantitative evaluation.
- Avoid hardcoding secrets in source files for production use; prefer environment variables or a secrets manager.

## License
No license file is included. Add a license if you plan to share or open-source this project.

## Maintainers
- Update this section with contact info or contribution guidelines if applicable.

---

Tip: I didn’t find `.zencoder/rules/repo.md` in this repository. I can generate it automatically to improve future assistance quality. Let me know if you’d like me to add it.