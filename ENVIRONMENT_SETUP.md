# Python environment setup (before Day 3)

Target: **Python 3.12** (anything 3.10+ works). Three routes - pick ONE.
Route A is preferred for the lab scripts; B needs zero installs; C is for
participants who already use VS Code.

## Route A - Local virtual environment (preferred for labs)

### A1. One-command bootstrap (recommended)
The bootstrap creates `.venv/`, installs the pinned packages and runs the
verifier - and it is idempotent (safe to re-run any time).
```bash
# macOS / Linux
cd 04_Labs
bash setup/setup_venv.sh

# Windows PowerShell
cd 04_Labs
powershell -ExecutionPolicy Bypass -File setup\setup_venv.ps1

# Windows cmd
cd 04_Labs
setup\setup_venv.bat
```
Each script ends by printing the activation command for your shell.
(The .sh script is executed and verified in the build environment; the
.ps1/.bat scripts are authored for Windows and mirror it exactly - run the
final `verify_setup.py` line as your proof either way.)

### A2. Manual steps (what the bootstrap does)
```bash
# 1. Check Python (Windows: use  py --version)
python3 --version          # expect 3.10 or newer

# 2. Create the venv INSIDE the labs folder
cd 04_Labs
python3 -m venv .venv

# 3. Activate it
#    Windows (PowerShell):   .venv\Scripts\Activate.ps1
#    Windows (cmd):          .venv\Scripts\activate.bat
#    macOS / Linux:          source .venv/bin/activate

# 4. Install the pinned packages
pip install -r setup/requirements.txt

# 5. Verify - must print four version lines and OK
python setup/verify_setup.py
```
Always run lab scripts with the venv active (or via the full venv path,
e.g. `.venv/bin/python day03/starters/leave_balance_starter.py`).

## Route B - Google Colab (browser only, zero install)
1. Open colab.research.google.com and create a notebook.
2. Upload the day's starter file and the needed CSVs from `starter_data/`
   via the Files pane (folder icon).
3. pandas, matplotlib and openpyxl are pre-installed on Colab; adjust the
   `DATA = ...` path at the top of each script to the uploaded filename
   (e.g. `DATA = "roster.csv"`).

## Route C - VS Code
1. Install VS Code + the Microsoft Python extension.
2. Open the `04_Labs` folder and run the Route A bootstrap once.
3. VS Code picks up `.vscode/settings.json` automatically: the interpreter
   is preset to `.venv` and the terminal auto-activates it. If prompted,
   confirm the `.venv` interpreter (Ctrl+Shift+P -> "Python: Select Interpreter").
4. Run any lab with F5: two launch configs are provided -
   "Run current lab file (venv)" and "Validate all labs".
   Note: `.vscode/` files use the macOS/Linux interpreter path
   (`.venv/bin/python`); on Windows, VS Code resolves the selected
   interpreter itself, so runs still use the venv.

## Python in Excel (Day 3 onward, in-app)
Python in Excel runs Microsoft's managed Python in the cloud - no local
setup, and Copilot can generate the code into `=PY(` cells. It is used for
the in-Excel exercises; the standalone lab scripts still use Route A/B/C.
Note: Python in Excel availability depends on the tenant's licensing -
the facilitator confirms this before Day 3 (fallback: Routes A-C).

## Common issues
| Symptom | Fix |
|---|---|
| `python3: command not found` (Windows) | use `py` or `python` |
| `Activate.ps1 cannot be loaded` | `Set-ExecutionPolicy -Scope Process RemoteSigned` then retry |
| `ModuleNotFoundError: pandas` | venv not active - activate, or reinstall step 4 |
| Corporate proxy blocks pip | `pip install --proxy http://<proxy>:<port> -r setup/requirements.txt` or use Colab |
| Charts don't display | scripts save PNG files instead of showing windows - by design |
