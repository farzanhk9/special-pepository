import os
import datetime
import json
from collections import Counter

DATA_FILE = "mood_log.json"

MOODS = {
    "1": "😊 Happy",
    "2": "😐 Neutral",
    "3": "😔 Sad",
    "4": "😡 Angry",
    "5": "😴 Tired",
    "6": "🤩 Excited"
}

def load_data():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_data(data):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=4)

def log_mood():
    print("\nHow are you feeling today?")
    for key, mood in MOODS.items():
        print(f"{key}. {mood}")
    choice = input("Choose a number: ")
    if choice in MOODS:
        entry = {
            "date": str(datetime.date.today()),
            "mood": MOODS[choice]
        }
        data.append(entry)
        save_data(data)
        print(f"✅ Mood logged: {MOODS[choice]}")
    else:
        print("⚠️ Invalid choice.")

def view_summary():
    if not data:
        print("📭 No mood data yet.")
        return
    print("\n📊 Weekly Mood Summary:")
    last_7 = [entry["mood"] for entry in data[-7:]]
    counts = Counter(last_7)
    for mood, count in counts.items():
        print(f"{mood}: {count} days")

def menu():
    while True:
        print("\n==== DAILY MOOD TRACKER ====")
        print("1. Log today's mood")
        print("2. View weekly summary")
        print("3. Exit")
        choice = input("Choose an option: ")
        
        if choice == "1":
            log_mood()
        elif choice == "2":
            view_summary()
        elif choice == "3":
            break
        else:
            print("⚠️ Invalid choice. Try again.")

data = load_data()
menu()
