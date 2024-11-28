# Rock Paper Scizors

## Wanna make multiplayer game of paper rock scissors in py, possible?

Yes, it's absolutely possible to create a multiplayer rock-paper-scissors game in Python! Here’s an overview of how you can approach it:

### **Features**

1. **Multiplayer**:

    - Use **sockets** for communication between players (client-server model).
    - A server handles connections and relays game data.
    - Clients send their choices to the server.

2. **Game Logic**:

    - Compare the choices of the players (rock beats scissors, scissors beats paper, paper beats rock).
    - Declare the winner.

3. **Simple UI**:
    - Use a **text-based interface** or expand it later with a GUI (e.g., Tkinter, PyGame).

---

### **Steps to Build It**

#### 1. **Server Code**

This handles the players and processes their moves.

```python
import socket
import threading

# Server Configuration
HOST = '127.0.0.1'  # Localhost
PORT = 5555

# Store player connections and choices
players = []
choices = {}

def handle_client(conn, addr):
    print(f"Player connected: {addr}")
    conn.sendall(b"Welcome to Rock-Paper-Scissors!\n")

    while True:
        try:
            # Receive player's choice
            choice = conn.recv(1024).decode().strip().lower()
            if choice not in ['rock', 'paper', 'scissors']:
                conn.sendall(b"Invalid choice. Choose rock, paper, or scissors.\n")
                continue

            choices[addr] = choice
            conn.sendall(b"Choice received! Waiting for opponent...\n")

            # Wait for both players
            if len(choices) == 2:
                # Compare choices
                result = process_game(choices)
                for player in players:
                    player.sendall(result.encode())
                break
        except:
            print(f"Player {addr} disconnected.")
            players.remove(conn)
            conn.close()
            break

def process_game(choices):
    [(addr1, choice1), (addr2, choice2)] = choices.items()
    if choice1 == choice2:
        return "It's a tie!"
    if (choice1 == 'rock' and choice2 == 'scissors') or \
       (choice1 == 'scissors' and choice2 == 'paper') or \
       (choice1 == 'paper' and choice2 == 'rock'):
        return f"Player {addr1} wins with {choice1}!"
    return f"Player {addr2} wins with {choice2}!"

# Server Setup
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((HOST, PORT))
server.listen(2)  # Limit to 2 players
print("Server started, waiting for players...")

while len(players) < 2:
    conn, addr = server.accept()
    players.append(conn)
    threading.Thread(target=handle_client, args=(conn, addr)).start()
```

---

#### 2. **Client Code**

This connects to the server and allows the player to make a choice.

```python
import socket

# Server Configuration
HOST = '127.0.0.1'
PORT = 5555

# Connect to Server
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((HOST, PORT))

while True:
    data = client.recv(1024).decode()
    print(data)
    if "Choose" in data:
        choice = input("Enter your choice (rock, paper, scissors): ").strip().lower()
        client.sendall(choice.encode())
    elif "wins" in data or "tie" in data:
        print(data)
        break
```

---

### **How It Works**

1. **Server**:

    - Waits for two players to connect.
    - Receives each player’s choice and determines the winner.
    - Sends the result back to both players.

2. **Client**:
    - Connects to the server.
    - Allows the user to input their choice.
    - Displays the game result.

---

### **Running the Game**

1. Run the **server** script first.
2. Start two instances of the **client** script (simulating two players).
3. Each player inputs their choice, and the server announces the winner.

---

### **Possible Enhancements**

-   Add more players and a tournament system.
-   Use a GUI for a better user experience.
-   Deploy the server on a public IP for online play.
-   Save match history to a file or database.

Would you like help expanding or refining this?
