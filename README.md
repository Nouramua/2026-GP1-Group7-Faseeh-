# Faseeh (فصيح) — Arabic Text Readability Classification & Simplification

Introduction

Much of the written Arabic content available today is too complex for a large group of readers, including school students, non-native speakers, and people with reading difficulties. Faseeh addresses this by combining two NLP tasks in one pipeline:

1- Readability classification — predicts how difficult an Arabic sentence is (levels 1–4).

2- Text simplification — rewrites a sentence so it is easier to understand.

Arabic text → Readability Classifier → Text Simplifier → Simplified text

The goal is to give students, educators, and general readers a tool that can assess and reduce the linguistic complexity of Arabic text.


Technologies Used

-Language: Python 3

-Environment: Google Colab (Jupyter notebooks)

-Classical ML: scikit-learn (TF-IDF, SVM, Random Forest, Decision Tree, XGBoost)

-Deep Learning: MSE Regression, Weighted Cross-Entropy, CORAL

# Faseeh website — Release 1

Faseeh (فصيح) is a website that shows how difficult an Arabic text is to read and simplifies it to an easier level.

**Release 1 includes:**
- Create an account, log in, log out and reset a forgotten password (Firebase)
- My Account: change email and delete the account
- «نقطة» (Nuqta) help on every page
- The Workspace: analysis and simplification are shown with **sample results**; the AI models are not connected yet (Release 2)

## What is in this folder

| Folder / file | What it is |
|---|---|
| `frontend/` | The website (React + TypeScript) |
| `backend/` | The PHP server that talks to Firebase |
| `firestore.rules` | Firestore security rules (pasted into Firebase once) |

---

## 1. Install these programs

| Program | Version | Check |
|---|---|---|
| Node.js | 20 or newer | `node -v` |
| PHP | 8.3 or newer | `php -v` |
| Composer | 2 | `composer --version` |

PHP needs these extensions turned on in `php.ini`: `openssl`, `sodium`, `curl`, `mbstring`.

## 2. Install the project

Open a terminal in this folder and run:

```bash
cd frontend
npm ci
cd ../backend
composer install
```

Then, still in `backend`, copy the settings template:

- Windows PowerShell: `Copy-Item .env.example .env`
- macOS / Linux / Git Bash: `cp .env.example .env`

## 3. Connect Firebase

In the [Firebase console](https://console.firebase.google.com):

1. Create a project (or open the team's project).
2. **Authentication → Sign-in method:** turn on **Email/Password**. Then **Templates → Password reset → Customize action URL:** enter `http://localhost:5173/reset-password`.
3. **Firestore Database → Create database** (production mode). In the **Rules** tab, paste the contents of `firestore.rules` and click **Publish**.
4. **Project settings → Service accounts → Generate new private key.** Save the file **outside this folder**.

Then open `backend/.env` and fill in:

| Setting | Value |
|---|---|
| `FIREBASE_PROJECT_ID` | Your project ID (Project settings → General) |
| `FIREBASE_CREDENTIALS` | The full path to the key file from step 4 |

Leave the other settings as they are.

More detail: `docs/FIREBASE_SETUP.md`.

## 4. Run the website

Use **two terminals**.

**Terminal 1 — backend:**

```bash
cd backend
php -S 127.0.0.1:8080 -t public public/index.php
```

**Terminal 2 — frontend:**

```bash
cd frontend
npm run dev
```

On Windows PowerShell, use `npm.cmd run dev` if `npm` is blocked.

Open **http://localhost:5173** in your browser. Stop either terminal with **Ctrl + C**.

## 5. Run the tests (optional)

| Test | Command |
|---|---|
| Backend tests (no Firebase needed) | `cd backend` then `php vendor/bin/phpunit --testsuite unit` |
| Build the website | `cd frontend` then `npm run build` |
| Browser tests | See `backend/tests/Browser/README.md` |

## If something goes wrong

| Problem | Fix |
|---|---|
| `vite` or `npm` errors | Run `npm ci` inside `frontend` |
| `vendor/autoload.php` not found | Run `composer install` inside `backend` |
| Account pages say «الخادم غير مهيّأ بعد» | `backend/.env` is missing a value; the backend terminal names it |
| Login or sign-up fails with a server error | Check the project ID, the key file path, and that Email/Password is turned on |
| The page opens but nothing works | Make sure **both** terminals are running |
