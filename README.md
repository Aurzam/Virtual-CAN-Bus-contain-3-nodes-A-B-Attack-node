# Virtual CAN Bus System - Complete Documentation

##  Table of Contents
1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Features](#features)
4. [Prerequisites](#prerequisites)
5. [Installation](#installation)
6. [Running the System](#running-the-system)
7. [File Structure](#file-structure)
8. [Configuration](#configuration)
9. [Message Formats](#message-formats)
10. [Security Demonstration](#security-demonstration)
11. [Troubleshooting](#troubleshooting)
12. [Extending the System](#extending-the-system)

---

##  Project Overview

A complete virtual implementation of a CAN (Controller Area Network) bus system with distributed nodes, demonstrating both protected and non-protected communication. The system simulates a real-world automotive/industrial network where multiple nodes communicate over a shared bus, with an attacker trying to intercept communications.

**Purpose**: To demonstrate the importance of encryption in CAN bus systems by showing how:
- Non-protected messages can be easily read by attackers
- Protected messages remain secure even when intercepted
- Different nodes can securely communicate using shared encryption keys

---

##  System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                     VIRTUAL CAN BUS ECOSYSTEM                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                 CENTRAL CAN BUS SERVER                      │    │
│  │                    (can_bus.py:5000)                        │    │
│  │  ┌─────────────────────────────────────────────────────┐    │    │
│  │  │                    Message Queue                    │    │    │
│  │  │  • Routes all messages                             │    │    │
│  │  │  • Maintains message history                       │    │    │
│  │  │  • Broadcasts to all connected nodes               │    │    │
│  │  └─────────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                          ▲         ▲         ▲                      │
│                          │         │         │                      │
│  ┌──────────────┐  ┌─────┴─────┐  │  ┌──────┴──────┐  ┌─────────────┐
│  │   Node A     │  │  Node B   │  │  │ Attack Node │  │ (Future)    │
│  │  (node_a.py) │  │(node_b.py)│  │  │(attack_node.│  │  Node C     │
│  ├──────────────┤  ├───────────┤  │  ├─────────────┤  ├─────────────┤
│  │ • A1: a1     │  │ • B1: b1  │  │  │ • Sniffs    │  │ • Extensible│
│  │ • A2: a2     │  │ • B2: b2  │  │  │   all       │  │   for more  │
│  │              │  │           │  │  │   traffic   │  │   nodes     │
│  └──────────────┘  └───────────┘  │  └─────────────┘  └─────────────┘
│        TCP Socket        TCP Socket│        TCP Socket       TCP Socket
│        Connection        Connection│        Connection       Connection
└─────────────────────────────────────────────────────────────────────┘

                          NETWORK COMMUNICATION
                          • TCP/IP Protocol
                          • JSON Message Format
                          • AES-128 Encryption
                          • Real-time Broadcasting
```

### Communication Flow:
1. **Node A** sends messages on paths `a1` (non-protected) and `a2` (protected)
2. **Node B** sends messages on paths `b1` (non-protected) and `b2` (protected)
3. **CAN Bus Server** receives all messages and broadcasts them to all connected nodes
4. **All nodes** receive all messages but can only decrypt those intended for them
5. **Attack Node** sees all traffic but cannot read encrypted messages

---

##  Features

###  Core Features
- **Distributed Architecture**: Each node runs as a separate process/terminal
- **Real Network Communication**: Uses TCP sockets for inter-process communication
- **AES-128 Encryption**: Military-grade encryption for protected messages
- **Message Routing**: Central bus server routes messages between nodes
- **Real-time Monitoring**: Live display of all network activity
- **Comprehensive Statistics**: Each node maintains and displays its own statistics
- **Modular Design**: Easy to add new nodes or modify existing ones

###  Security Features
- **Protected Messages**: Encrypted with AES-128-CBC
- **Non-protected Messages**: Plain text for demonstration purposes
- **Attack Simulation**: Shows exactly what an attacker can see
- **Key Management**: Shared secret key between legitimate nodes
- **Encryption Verification**: Nodes verify they can decrypt each other's messages

###  Monitoring Features
- **Live Traffic Display**: See messages as they're sent and received
- **Periodic Statistics**: Automatic statistics every 10-15 seconds
- **Traffic Analysis**: Breakdown by sender, type, and path
- **Security Analysis**: Shows percentage of protected vs non-protected messages
- **Message History**: Each node maintains history of all received messages

---

##  Prerequisites

### Software Requirements
- **Python 3.7 or higher**
- **pip** (Python package installer)
- **Terminal/Command Prompt** (multiple instances)
- **Network ports 5000-5003** available

### Python Libraries
The system requires:
- `pycryptodome` for AES encryption
- All other dependencies are Python standard library

---

##  Installation

### Step-by-Step Installation

1. **Create Project Directory**
```bash
mkdir virtual-can-bus
cd virtual-can-bus
```

2. **Create Project Files**
Create all the files from the provided code:
- `config.py`
- `crypto_utils.py`
- `can_bus.py`
- `message_broker.py`
- `node_a.py`
- `node_b.py`
- `attack_node.py`
- `main.py`
- `requirements.txt`
- `README.md`
- `run_all.bat` (Windows)
- `run_all.sh` (Linux/Mac)

3. **Install Dependencies**
```bash
pip install -r requirements.txt
```

4. **Verify Installation**
```bash
python --version
# Should show Python 3.7 or higher

python -c "from Crypto.Cipher import AES; print('Crypto library OK')"
# Should print "Crypto library OK"
```

---

##  Running the System

### Method 1: Automated Script (Recommended)

#### Windows Users:
```bash
run_all.bat
```
This will:
1. Open 4 separate command prompt windows
2. Start the CAN Bus server in the first window
3. Start Node A in the second window
4. Start Node B in the third window
5. Start Attack Node in the fourth window
6. Display instructions in the original window

#### Linux/Mac Users:
```bash
# Make script executable
chmod +x run_all.sh

# Run the script
./run_all.sh
```

### Method 2: Using Main Controller

```bash
# Run all nodes in separate terminals
python main.py --all

# Run specific nodes
python main.py --node bus     # Start only CAN Bus server
python main.py --node a       # Start only Node A
python main.py --node b       # Start only Node B
python main.py --node attack  # Start only Attack Node
```

### Method 3: Manual Execution (Advanced)

Open **4 separate terminal windows** and run:

**Terminal 1 - CAN Bus Server:**
```bash
python can_bus.py
```

**Terminal 2 - Node A:**
```bash
python node_a.py
```

**Terminal 3 - Node B:**
```bash
python node_b.py
```

**Terminal 4 - Attack Node:**
```bash
python attack_node.py
```

**Wait 2 seconds between starting each node** to allow proper connection.

---

##  File Structure

```
virtual-can-bus/
│
├──  config.py                 # Global configuration
│   ├── AES encryption key
│   ├── Message intervals
│   ├── Network ports
│   ├── Path definitions
│   └── Logging settings
│
├──  crypto_utils.py           # Encryption utilities
│   ├── AES-128 encryption
│   ├── AES-128 decryption
│   ├── Key generation
│   └── Key validation
│
├──  can_bus.py                # Central CAN bus server
│   ├── TCP server implementation
│   ├── Message routing
│   ├── Client management
│   ├── Statistics collection
│   └── Broadcast functionality
│
├──  message_broker.py         # Node communication handler
│   ├── Connection management
│   ├── Message sending/receiving
│   ├── JSON serialization
│   └── Error handling
│
├──  node_a.py                 # Node A implementation
│   ├── Sends A1 (non-protected) messages
│   ├── Sends A2 (protected) messages
│   ├── Receives messages from Node B
│   ├── Statistics display
│   └── Message decryption
│
├──  node_b.py                 # Node B implementation
│   ├── Sends B1 (non-protected) messages
│   ├── Sends B2 (protected) messages
│   ├── Receives messages from Node A
│   ├── Statistics display
│   └── Message decryption
│
├──  attack_node.py           # Attack node implementation
│   ├── Sniffs all traffic
│   ├── Displays encrypted vs plain text
│   ├── Security analysis
│   ├── Traffic statistics
│   └── Cannot decrypt protected messages
│
├──  main.py                   # System controller
│   ├── Command-line interface
│   ├── Process management
│   ├── Automated startup
│   └── User interaction
│
├──  requirements.txt          # Python dependencies
├──  README.md                 # This documentation
├──  run_all.bat              # Windows automation script
└──  run_all.sh               # Linux/Mac automation script
```

---

##  Configuration

### Key Configuration Files

#### 1. **config.py** - Main Configuration
```python
# AES Key (16 bytes for AES-128)
AES_KEY = b'MySecretKey12345!'  # Change this for production!

# Message intervals (seconds)
INTERVALS = {
    'A1': 3,  # Node A non-protected - every 3 seconds
    'A2': 4,  # Node A protected - every 4 seconds
    'B1': 3,  # Node B non-protected - every 3 seconds
    'B2': 4,  # Node B protected - every 4 seconds
}

# Message paths
PATHS = {
    'A1': 'a1',  # Node A non-protected path
    'A2': 'a2',  # Node A protected path
    'B1': 'b1',  # Node B non-protected path
    'B2': 'b2',  # Node B protected path
}

# Network configuration
BROKER_HOST = 'localhost'
BROKER_PORT = 5000  # CAN Bus server port
```

#### 2. **Customizing Message Content**
To change message content, modify the message creation in:
- `node_a.py`: Lines creating A1 and A2 messages
- `node_b.py`: Lines creating B1 and B2 messages

Example:
```python
# In node_a.py, change this line:
plain_text = f"My name is node A and the data is (A2 and C)"
# To:
plain_text = f"Temperature reading: {temperature_value} degrees"
```

---

##  Message Formats

### Non-Protected Message Format (Plain Text)
```json
{
    "type": "non-protected",
    "path": "a1",
    "data": "My name is node A and the data is (A1) and [0]",
    "sender": "Node A",
    "counter": 0,
    "timestamp": "2024-01-15T10:30:45.123456",
    "message_id": 1,
    "sender_id": "node_a",
    "sender_name": "Node A",
    "bus_timestamp": "10:30:45.123"
}
```

### Protected Message Format (Encrypted)
```json
{
    "type": "protected",
    "path": "a2",
    "data": "U2FsdGVkX19DpzH8wQ4p7YhVz6G9KbLmNoPqRsT...",  # Base64 encrypted
    "original_data": "My name is node A and the data is (A2 and C)",
    "sender": "Node A",
    "counter": 0,
    "timestamp": "2024-01-15T10:30:46.123456",
    "encryption": "AES-128-CBC",
    "message_id": 2,
    "sender_id": "node_a",
    "sender_name": "Node A",
    "bus_timestamp": "10:30:46.123"
}
```

### What the Attack Node Sees:
- **Non-protected messages**: Full plain text content
- **Protected messages**: Encrypted gibberish (Base64 string)

---

##  Security Demonstration

### Scenario 1: Non-Protected Communication
```
Node A sends: "My name is node A and the data is (A1) and [0]"
↓
CAN Bus broadcasts to all nodes
↓
Attack Node sees: "My name is node A and the data is (A1) and [0]"  ← COMPROMISED!
```

### Scenario 2: Protected Communication
```
Node A sends: "My name is node A and the data is (A2 and C)"
↓
AES-128 Encryption
↓
Encrypted: "U2FsdGVkX19DpzH8wQ4p7YhVz6G9KbLmNoPqRsT..."
↓
CAN Bus broadcasts to all nodes
↓
Attack Node sees: "U2FsdGVkX19DpzH8wQ4p7YhVz6G9KbLmNoPqRsT..."  ← ENCRYPTED!
↓
Node B receives and decrypts: "My name is node A and the data is (A2 and C)"  ← SECURE!
```

### Visual Demonstration in Terminals:

**Node A Terminal:**
```
[14:30:45] Node A: Sent A1 on path a1
[14:30:46] Node A: Sent A2 (PROTECTED) on path a2
[14:30:47] Node A: Received from Node B -> My name is node B and the data is (B1) and [0]
[14:30:48] Node A: Received PROTECTED from Node B -> My name is node B and the data is (B2 and C)
```

**Attack Node Terminal:**
```
[14:30:45] Attack Node: SNIFFED from Node A on a1
    Plain text: My name is node A and the data is (A1) and [0]
[14:30:46] Attack Node: SNIFFED PROTECTED from Node A on a2
    Encrypted: U2FsdGVkX19DpzH8wQ4p7YhVz6G9KbLmNoPqRsT...
[14:30:47] Attack Node: SNIFFED from Node B on b1
    Plain text: My name is node B and the data is (B1) and [0]
```

---

##  Expected Output

### CAN Bus Server Output:
```
============================================================
VIRTUAL CAN BUS STARTED
Listening on localhost:5000
============================================================

[CAN BUS] Node connected: Node A (node_a)
[CAN BUS] Node connected: Node B (node_b)
[CAN BUS] Node connected: Attack Node (attack_node)
Message 1: Node A -> a1
Message 2: Node A -> a2
Message 3: Node B -> b1
Message 4: Node B -> b2

============================================================
CAN BUS STATISTICS
============================================================
Connected nodes: 3
Total messages: 42
Recent messages by type: {'non-protected': 21, 'protected': 21}
Recent messages by sender: {'Node A': 21, 'Node B': 21}
============================================================
```

### Node A Output:
```
============================================================
STARTING Node A
============================================================
Message Configuration:
  - A1: Non-protected on path 'a1' every 3s
  - A2: Protected (AES-128) on path 'a2' every 4s
============================================================

[14:30:45] Node A: Sent A1 on path a1
[14:30:46] Node A: Received from Node B -> My name is node B and the data is (B1) and [0]
[14:30:47] Node A: Sent A2 (PROTECTED) on path a2
[14:30:48] Node A: Received PROTECTED from Node B -> My name is node B and the data is (B2 and C)

============================================================
Node A STATISTICS
============================================================
Messages sent:
  A1 (non-protected): 5
  A2 (protected): 4
Messages received: 9
Recent messages by sender: {'Node B': 9}
============================================================
```

### Attack Node Output:
```
============================================================
STARTING Attack Node
============================================================
Mode: SNIFFING ALL TRAFFIC
Can see both protected and non-protected messages
Cannot decrypt protected messages
============================================================

[14:30:45.123] Attack Node: SNIFFED from Node A on a1
    Plain text: My name is node A and the data is (A1) and [0]
[14:30:46.456] Attack Node: SNIFFED PROTECTED from Node A on a2
    Encrypted: U2FsdGVkX19DpzH8wQ4p7YhVz6G9KbLmNoPqRsT...

============================================================
Attack Node - SNIFFING STATISTICS
============================================================
Total messages sniffed: 42
By Sender:
  Node A: 21
  Node B: 21
By Type:
  non-protected: 21
  protected: 21
By Path:
  a1: 10
  a2: 11
  b1: 10
  b2: 11
============================================================

============================================================
Attack Node - SECURITY ANALYSIS
============================================================
Protected messages: 21 (50.0%)
Non-protected messages: 21 (50.0%)

Protected messages appear as ENCRYPTED GIBBERISH
Attack node CANNOT read the content

Non-protected messages are READABLE
Attack node can see all content

Recommendation: Use protection for sensitive data
============================================================
```

---

##  Troubleshooting

### Common Issues and Solutions:

#### Issue 1: "Port already in use"
```
Error: [Errno 48] Address already in use
```
**Solution:**
```bash
# Find process using port 5000
lsof -i :5000  # Linux/Mac
netstat -ano | findstr :5000  # Windows

# Kill the process
kill <PID>  # Linux/Mac
taskkill /PID <PID> /F  # Windows
```

#### Issue 2: "ModuleNotFoundError: No module named 'Crypto'"
**Solution:**
```bash
# Install the correct package
pip install pycryptodome

# Or if using pip3
pip3 install pycryptodome
```

#### Issue 3: Nodes can't connect to CAN Bus
**Solution:**
1. Make sure CAN Bus server is running first
2. Wait 2 seconds before starting nodes
3. Check firewall isn't blocking port 5000
4. Verify all nodes use same host (localhost)

#### Issue 4: Attack Node shows "Failed to connect"
**Solution:**
```bash
# Start components in correct order:
1. python can_bus.py
2. Wait 2 seconds
3. python node_a.py
4. python node_b.py
5. python attack_node.py
```

#### Issue 5: "Encryption/Decryption failed"
**Solution:**
1. Ensure all legitimate nodes use the SAME AES key
2. Check `config.py` AES_KEY is exactly 16 bytes
3. Restart all nodes if key was changed

---

##  Extending the System

### Adding a New Node (Node C)

1. **Create `node_c.py`:**
```python
"""
Node C - Example new node
"""
import threading
import time
import logging
from datetime import datetime
from message_broker import MessageBroker
from crypto_utils import CryptoUtils
from config import *

logging.basicConfig(level=LOG_LEVEL, format=LOG_FORMAT)
logger = logging.getLogger(__name__)

class NodeC:
    def __init__(self):
        self.node_id = 'node_c'
        self.node_name = 'Node C'
        self.broker = None
        self.running = False
        self.crypto = CryptoUtils()
        
    def start(self):
        # Similar to node_a.py but with C1, C2 messages
        pass
    
    def send_c1_messages(self):
        # Non-protected messages on path c1
        pass
    
    def send_c2_messages(self):
        # Protected messages on path c2
        pass

def main():
    node = NodeC()
    # Start the node
    pass

if __name__ == "__main__":
    main()
```

2. **Update `config.py`:**
```python
# Add to INTERVALS:
INTERVALS = {
    # ... existing intervals ...
    'C1': 5,  # Node C non-protected
    'C2': 6,  # Node C protected
}

# Add to PATHS:
PATHS = {
    # ... existing paths ...
    'C1': 'c1',
    'C2': 'c2',
}
```

3. **Update automation scripts** to include Node C

### Modifying Message Intervals
Edit `config.py`:
```python
INTERVALS = {
    'A1': 2,  # Changed from 3 to 2 seconds
    'A2': 3,  # Changed from 4 to 3 seconds
    'B1': 2,  # Changed from 3 to 2 seconds
    'B2': 3,  # Changed from 4 to 3 seconds
}
```

### Changing Encryption Algorithm
Edit `crypto_utils.py` to implement a different algorithm (e.g., AES-256).

---
