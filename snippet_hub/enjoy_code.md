# Enjoy Code

## Can we make py script that will help me to realize all of that? [Enjoy Code Theory](../coding/enjoy.md)

Absolutely! Let’s design a **Python script** that serves as a personal assistant to help you stay focused on the joy of coding while avoiding stress about productivity. Here's what the script can include:

---

### **Features**

1. **Daily Reminder to Focus on the Process**

    - Encouraging messages when you start and end your day.

2. **Session Tracker**

    - Logs what you enjoyed or learned during coding sessions, not the time spent.

3. **Prompt for Curiosity**

    - Provides random questions like _"What if you try X?"_ or _"How could you solve this differently?"_ to spark curiosity.

4. **Break and Ritual Manager**

    - Suggests breaks and simple rituals to stay relaxed, like _"Take a deep breath and enjoy some warm milk."_

5. **Minimal Progress Logger**

    - Tracks small wins with minimal effort, focusing on feelings of success rather than productivity metrics.

6. **Motivational Journal**
    - Allows you to record reflections on _why_ you love coding, which can be displayed randomly as reminders.

---

### **Script Example**

Here’s a starting point for the script:

```python
import random
import datetime
import json
import os

# File paths
LOG_FILE = "coding_journal.json"

# Sample prompts and messages
MOTIVATIONAL_QUOTES = [
    "The process is the reward—enjoy the journey!",
    "Small steps build big dreams. Celebrate each one!",
    "Coding is your playground—what will you explore today?",
    "Curiosity fuels creativity. Follow it!",
    "There’s no rush. Take your time to create something you love."
]

CURIOSITY_PROMPTS = [
    "What if you solve this problem in a completely new way?",
    "How would a beginner approach this?",
    "Can you simplify this part of the code?",
    "What happens if you add or remove this feature?",
    "Is there a fun twist you can add to this project?"
]

# Ensure the log file exists
if not os.path.exists(LOG_FILE):
    with open(LOG_FILE, "w") as f:
        json.dump([], f)

def add_log(entry):
    """Add an entry to the coding journal."""
    with open(LOG_FILE, "r") as f:
        logs = json.load(f)

    logs.append(entry)

    with open(LOG_FILE, "w") as f:
        json.dump(logs, f, indent=2)

def show_daily_reminder():
    """Show a motivational message."""
    print("\n✨ Daily Reminder ✨")
    print(random.choice(MOTIVATIONAL_QUOTES))
    print()

def log_session():
    """Log a coding session."""
    print("\n📝 Log Your Session")
    reflection = input("What did you enjoy or learn today? ")
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    add_log({"timestamp": timestamp, "reflection": reflection})
    print("Your session has been logged. Keep it up!")

def curiosity_prompt():
    """Show a curiosity prompt."""
    print("\n🤔 Curiosity Prompt")
    print(random.choice(CURIOSITY_PROMPTS))
    print()

def break_reminder():
    """Suggest a relaxing break."""
    print("\n☕ Break Time")
    print("Take a deep breath. Enjoy a warm drink or do something you love for a few minutes.")
    print()

def motivational_journal():
    """View your reflections and motivations."""
    with open(LOG_FILE, "r") as f:
        logs = json.load(f)

    print("\n📖 Your Reflections")
    for log in logs[-5:]:  # Show last 5 entries
        print(f"- [{log['timestamp']}] {log['reflection']}")

def main():
    """Main menu."""
    while True:
        print("\n--- Coding Joy Assistant ---")
        print("1. Show Daily Reminder")
        print("2. Log a Session")
        print("3. Show a Curiosity Prompt")
        print("4. Suggest a Break")
        print("5. View Motivational Journal")
        print("6. Exit")
        choice = input("Choose an option: ")

        if choice == "1":
            show_daily_reminder()
        elif choice == "2":
            log_session()
        elif choice == "3":
            curiosity_prompt()
        elif choice == "4":
            break_reminder()
        elif choice == "5":
            motivational_journal()
        elif choice == "6":
            print("Goodbye! Keep enjoying your projects.")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```

---

### **How to Use**

1. **Run the Script**  
   Save it as `coding_joy_assistant.py` and run it in your terminal:

    ```bash
    python coding_joy_assistant.py
    ```

2. **Log Your Progress**  
   Use the menu to log what you enjoyed or learned, reflect, or take a break.

3. **Customize It**  
   Add your own quotes, prompts, or rituals to make it personal.

---

Would you like to expand on any specific feature or refine the script?
