# LEGO Set Guessing Game

A fun and interactive LEGO set guessing game built with Vue 3 + Vite on the frontend and Python and FastAPI on the backend. The game displays a LEGO set image along with some metadata, and the user has to guess the set name.

## What the game looks like <3

<img src="lego-guessing-frontend/src/assets/Screenshot%202025-06-11%20at%2016.33.49.png" width="600"/>
<img src="lego-guessing-frontend/src/assets/Screenshot%202025-06-11%20at%2016.34.22.png" width="600"/>

## Features

- Displays LEGO set image, year, theme, and piece count
- Input validation (no empty guesses)
- Instant feedback: correct or wrong
- Clean and playful UI inspired by LEGO's visual style

## Tech Stack

- [Vue 3](https://vuejs.org/)
- [Vite](https://vitejs.dev/)
- CSS3
- Cera Pro font

## Getting Started

```bash
cd lego-guessing-frontend
npm install
npm run dev


---

## 📁 Project 2: `guess-lego-set-game` (assumed to be the backend)

```md
# 🧠 LEGO Set Guessing Game – Backend

A simple backend API built to serve random LEGO sets and validate user guesses. Designed to work with the `lego-guessing-frontend` Vue app.

## 📦 Features

- `GET /random-set`: Returns a random LEGO set with metadata
- `POST /guess`: Validates the user's guess
- CORS enabled for local frontend development

## ⚙️ Tech Stack

- Python 3
- FastAPI
- Uvicorn

## 🚀 Getting Started

```bash
cd guess-lego-set-game
pip install -r requirements.txt
uvicorn main:app --reload
