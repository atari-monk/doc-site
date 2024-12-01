Create py code. Gather cli input, put it to data model. Data model will gather information of electronic parts. They are quite diffrent. Data structure must be generic. What do u propose. I propose key value pairs object array.

A generic and flexible data model for electronic parts can use a **key-value pair object array**. This approach allows the storage of diverse properties without predefining specific fields for every part type. Each electronic part will be represented as an object (dictionary), and an array will hold these objects.

Here's a Python script to gather CLI input, store the information in a key-value pair structure, and manage it in memory. Later, it can be saved to or loaded from a JSON file for persistence.

### Code

```python
import json

class ElectronicPartsManager:
    def __init__(self):
        self.parts = []  # List to store electronic part objects

    def add_part(self, part_data):
        """Add a new part to the collection."""
        self.parts.append(part_data)

    def list_parts(self):
        """List all parts."""
        if not self.parts:
            print("No parts available.")
        else:
            for i, part in enumerate(self.parts, 1):
                print(f"Part {i}: {json.dumps(part, indent=2)}")

    def save_to_file(self, filename):
        """Save parts to a JSON file."""
        try:
            with open(filename, 'w') as f:
                json.dump(self.parts, f, indent=2)
            print(f"Parts saved to {filename}")
        except Exception as e:
            print(f"Error saving file: {e}")

    def load_from_file(self, filename):
        """Load parts from a JSON file."""
        try:
            with open(filename, 'r') as f:
                self.parts = json.load(f)
            print(f"Parts loaded from {filename}")
        except Exception as e:
            print(f"Error loading file: {e}")

def get_user_input():
    """Gather key-value data from the user."""
    part_data = {}
    print("Enter key-value pairs for the electronic part. Type 'done' when finished.")
    while True:
        key = input("Key: ").strip()
        if key.lower() == "done":
            break
        value = input("Value: ").strip()
        part_data[key] = value
    return part_data

def main():
    manager = ElectronicPartsManager()

    while True:
        print("\n1. Add a new part")
        print("2. List all parts")
        print("3. Save parts to file")
        print("4. Load parts from file")
        print("5. Exit")

        choice = input("Select an option: ").strip()
        if choice == "1":
            part = get_user_input()
            manager.add_part(part)
        elif choice == "2":
            manager.list_parts()
        elif choice == "3":
            filename = input("Enter filename to save to: ").strip()
            manager.save_to_file(filename)
        elif choice == "4":
            filename = input("Enter filename to load from: ").strip()
            manager.load_from_file(filename)
        elif choice == "5":
            print("Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```

### Key Features
1. **Generic Structure**: Each part is stored as a dictionary, allowing flexibility to accommodate different attributes for different parts.
2. **CLI Interaction**: Allows users to input parts interactively.
3. **Persistence**: Parts can be saved to and loaded from a JSON file.
4. **Scalability**: Easy to extend for validation, additional commands, or more complex features.

### Usage Example
1. Run the script.
2. Select "Add a new part" and input key-value pairs:
   ```
   Key: name
   Value: Resistor
   Key: resistance
   Value: 10k
   Key: tolerance
   Value: 5%
   Key: done
   ```
3. Save to a file or list all added parts.

This approach ensures flexibility and adaptability to different electronic part types.
