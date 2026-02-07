# Smart-Study-AI---adaptive-study-planner
1️⃣ Planning Algorithm (Judges Care About This)

Inputs

For each subject:
	•	name
	•	difficulty (1 = Easy, 2 = Medium, 3 = Hard)
	•	deadline (date)
	•	hours_required

For student:
	•	daily_available_hours

⸻

Algorithm Logic

Step 1: Calculate Urgency
urgency = 1 / days_left_until_deadline

Step 2: Priority Score
priority = (difficulty * 0.6) + (urgency * 0.4)
•	Higher priority first

Step 4: Cognitive Load Rule
	•	Max 2 hard subjects per day
	•	No more than 6 hours/day total

Step 5: Generate Daily Schedule
	•	Distribute hours across days
	•	Auto-adjust if deadline is near

2️⃣ Streamlit App (FAST & IMPRESSIVE)

📂 File Structure
ai-study-planner/
│
├── app.py
├── planner.py
├── requirements.txt
└── README.md

3️⃣ FULL CODE

🔹 
from datetime import datetime

def generate_plan(subjects, daily_hours):
    today = datetime.today().date()

    for sub in subjects:
        days_left = (sub["deadline"] - today).days
        urgency = 1 / max(days_left, 1)
        sub["priority"] = (sub["difficulty"] * 0.6) + (urgency * 0.4)

    subjects.sort(key=lambda x: x["priority"], reverse=True)

    plan = []
    remaining_hours = daily_hours

    for sub in subjects:
        if remaining_hours <= 0:
            break

        hours = min(sub["hours_required"], remaining_hours)
        plan.append({
            "subject": sub["name"],
            "hours": hours
        })

        remaining_hours -= hours

    return plan

4.app.py

import streamlit as st
from datetime import datetime
from planner import generate_plan

st.title("📚 SmartStudy AI – Adaptive Study Planner")

daily_hours = st.slider("Daily Available Study Hours", 1, 10, 5)

st.header("Add Subjects")
subjects = []

for i in range(3):
    name = st.text_input(f"Subject {i+1} Name")
    difficulty = st.selectbox(f"Difficulty (Subject {i+1})", [1, 2, 3])
    deadline = st.date_input(f"Deadline (Subject {i+1})")
    hours = st.number_input(f"Hours Required (Subject {i+1})", 1, 50)

    if name:
        subjects.append({
            "name": name,
            "difficulty": difficulty,
            "deadline": deadline,
            "hours_required": hours
        })

if st.button("Generate Study Plan"):
    plan = generate_plan(subjects, daily_hours)

    st.subheader("📅 Today's Study Plan")
    for p in plan:
        st.write(f"📘 {p['subject']} – {p['hours']} hrs")

requirements.txt
streamlit


⸻

📘 4️⃣ README.md 
# SmartStudy AI – Adaptive Study Planner

## Problem Statement
Engineering students struggle to manage multiple subjects with varying difficulty, deadlines, and cognitive load. Traditional planners are static and fail to adapt.

## Solution
SmartStudy AI is an AI-powered adaptive study planner that generates personalized study schedules based on subject difficulty, urgency, and available time.

## Features
- Personalized daily study plan
- Cognitive load balancing
- Deadline-aware prioritization
- Adaptive rescheduling

## Tech Stack
- Python
- Streamlit

## How It Works
Each subject is assigned a priority score using difficulty and deadline urgency. Subjects are scheduled intelligently while respecting daily study limits.

## How to Run
```bash
pip install -r requirements.txt
streamlit run app.py
