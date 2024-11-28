# Wellness

## Let's create a py cli loop that will use scientific methods to make me feel better pls

To create a Python CLI loop that uses scientific methods to improve your well-being, we can include features based on evidence-based practices like mindfulness, gratitude journaling, habit tracking, and physical activity prompts. Here's a script outline:

### Features

1. **Gratitude Journal**: Prompt the user to write three things they’re grateful for.
2. **Breathing Exercise**: Guide a short mindfulness session (e.g., "Breathe in for 4 seconds, hold for 4 seconds, breathe out for 6 seconds").
3. **Mood Tracker**: Allow the user to log their current mood on a scale of 1-10.
4. **Physical Activity Reminder**: Suggest a small exercise, like stretching or a short walk.
5. **Progress Summary**: Display logs and patterns over time.

### Code Implementation

```python
import json
import time
from datetime import datetime

# File for logs
LOG_FILE = "wellness_logs.json"

# Load existing logs or create new
try:
    with open(LOG_FILE, "r") as f:
        logs = json.load(f)
except FileNotFoundError:
    logs = {"gratitude": [], "mood": [], "exercise": []}

# Save logs
def save_logs():
    with open(LOG_FILE, "w") as f:
        json.dump(logs, f, indent=2)

# Main menu
def main_menu():
    print("\n=== Wellness CLI ===")
    print("1. Gratitude Journal")
    print("2. Breathing Exercise")
    print("3. Mood Tracker")
    print("4. Physical Activity Reminder")
    print("5. View Progress Summary")
    print("0. Exit")
    return input("Choose an option: ")

# Gratitude Journal
def gratitude_journal():
    print("\nGratitude Journal")
    for i in range(1, 4):
        gratitude = input(f"Write something you're grateful for ({i}/3): ")
        if gratitude:
            logs["gratitude"].append({"entry": gratitude, "timestamp": str(datetime.now())})
    save_logs()
    print("Gratitude logged. Feel the positivity!")

# Breathing Exercise
def breathing_exercise():
    print("\nBreathing Exercise")
    print("Follow the breathing instructions:")
    for _ in range(3):
        print("Breathe in... (4 seconds)")
        time.sleep(4)
        print("Hold... (4 seconds)")
        time.sleep(4)
        print("Breathe out... (6 seconds)")
        time.sleep(6)
    print("Breathing exercise complete. Notice the calmness.")

# Mood Tracker
def mood_tracker():
    mood = input("\nHow are you feeling right now? (Rate 1-10): ")
    try:
        mood = int(mood)
        if 1 <= mood <= 10:
            logs["mood"].append({"mood": mood, "timestamp": str(datetime.now())})
            save_logs()
            print("Mood logged.")
        else:
            print("Please enter a number between 1 and 10.")
    except ValueError:
        print("Invalid input. Please enter a number.")

# Physical Activity Reminder
def physical_activity_reminder():
    print("\nPhysical Activity Suggestion")
    suggestion = "Do 10 jumping jacks, stretch for 5 minutes, or take a short walk."
    print(suggestion)
    completed = input("Did you complete the activity? (yes/no): ").strip().lower()
    if completed == "yes":
        logs["exercise"].append({"activity": suggestion, "timestamp": str(datetime.now())})
        save_logs()
        print("Activity logged. Great job!")
    else:
        print("That's okay, try to move a little later.")

# View Progress Summary
def view_progress_summary():
    print("\nProgress Summary")
    print("Gratitude Entries:")
    for entry in logs["gratitude"]:
        print(f"- {entry['entry']} (Logged: {entry['timestamp']})")
    print("\nMood Logs:")
    for mood in logs["mood"]:
        print(f"- Mood: {mood['mood']} (Logged: {mood['timestamp']})")
    print("\nExercise Logs:")
    for exercise in logs["exercise"]:
        print(f"- {exercise['activity']} (Logged: {exercise['timestamp']})")

# Main loop
def main():
    while True:
        choice = main_menu()
        if choice == "1":
            gratitude_journal()
        elif choice == "2":
            breathing_exercise()
        elif choice == "3":
            mood_tracker()
        elif choice == "4":
            physical_activity_reminder()
        elif choice == "5":
            view_progress_summary()
        elif choice == "0":
            print("Exiting Wellness CLI. Take care!")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```

### How It Helps

-   **Gratitude Journal**: Boosts positivity and mindfulness.
-   **Breathing Exercise**: Reduces stress and calms the mind.
-   **Mood Tracker**: Identifies emotional patterns and trends.
-   **Physical Activity Reminder**: Encourages movement, enhancing mood and health.
-   **Progress Summary**: Visualizes improvements and reinforces consistency.

Would you like additional features or customization?
