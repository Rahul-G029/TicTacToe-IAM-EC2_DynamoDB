# 🕹️ Tic-Tac-Toe with AWS (EC2 + DynamoDB)

This project is a simple cloud-based **Tic-Tac-Toe game** using **Python Flask**, hosted on an **AWS EC2 instance**, and stores game data in **AWS DynamoDB**.

---

## 🌟 Features

* Create a new game and invite another player.
* Accept or reject game invitations.
* Play with turn-based logic and automatic result checking.
* All game data is stored and managed in DynamoDB.

---

## ☁️ AWS Architecture

| Component        | Description                                                                       |
| ---------------- | --------------------------------------------------------------------------------- |
| **EC2 Instance** | Hosts the Flask application on Amazon Linux 2.                                    |
| **DynamoDB**     | NoSQL database to store game state, players, and results.                         |
| **IAM Role**     | Used to securely authorize EC2 to access DynamoDB without hardcoding credentials. |

---

## ⚙️ Application Components

* `application.py`: Entry point that launches the Flask web service.
* `game_controller.py`: Handles the game logic and board updates.
* `connection_manager.py`: Connects to DynamoDB and performs CRUD operations.
* `templates/`: Contains HTML pages rendered by Flask.
* `config.ini`: Stores table and region configuration.

---

## 🧩 Game Workflow

1. **Create New Game**
   Create a game with a unique ID, creator, and invitee. Game is stored in DynamoDB.

2. **Accept/Reject Game Invite**
   Invitee can accept or reject the game invite. The game state updates accordingly.

3. **Update Game Board**
   Players take turns. Each move updates the board and switches turns.

4. **Check Game Result**
   Game logic checks for a win, tie, or ongoing game and updates status in DynamoDB.

---

## 🛠️ Installation Steps (Same as Provided)

### Install Development Tools and Dependencies

Installs essential development tools and libraries needed for compiling and building software.

```bash
sudo yum groupinstall -y "Development Tools"
```

Installs development headers and libraries for OpenSSL, bzip2, and libffi, required for Python.

```bash
sudo yum install -y openssl-devel bzip2-devel libffi-devel
```

---

### Install Python 2.7.18

Downloads Python 2.7.18, extracts it, configures the build with optimizations enabled, and installs it without replacing the system-provided Python.

```bash
cd /usr/src
sudo wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz
sudo tar xzf Python-2.7.18.tgz
cd Python-2.7.18
sudo ./configure --enable-optimizations
sudo make altinstall
```

Verifies the installed Python version.

```bash
python2.7 -V
```

---

### Install pip for Python 2.7

Downloads and installs pip for Python 2.7.

```bash
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
python get-pip.py
```

---

### Install Python Packages

Installs Flask, Boto (for AWS SDK), and configparser Python packages using pip.

```bash
pip install Flask
pip install boto
pip install configparser
```

---

### Install Git

Installs Git, a version control system.

```bash
yum install git -y
```

---

### Clone the TicTacToe Project Repository

Clones the TicTacToe project repository from GitHub into the `/home/ec2-user/` directory.

```bash
cd /home/ec2-user/
git clone https://github.com/avizway1/tictactoe-with-DynamoDB.git
```

---

### Create the Configuration File

Create a file named `config.ini` in the project directory with the following content. Make sure to adjust the region as needed:

```ini
[Settings]
TableName = Games
Region = us-east-1

[dynamodb]
region=us-east-1
endpoint=dynamodb.ap-south-2.amazonaws.com
```

---

### IAM Role and Metadata Settings

* Attach an **IAM role** to the EC2 instance that has valid access to **create/read/write** DynamoDB tables.
* While launching the EC2 instance, enable **Instance Metadata Service** for both **Version 1 and Version 2**.

  > The app uses the metadata URL to obtain temporary AWS credentials.

---

## 🚀 Run the Application

Navigate to the project folder and run the following command:
**(Ensure port 5000 is open in your EC2 security group.)**

```bash
python application.py --config config.ini --mode service --endpoint dynamodb.ap-south-2.amazonaws.com --serverPort 5000
```

Now open your browser and visit:

```
http://<your-ec2-public-ip>:5000/
```

You’ll see the Tic-Tac-Toe game running!

---

## ✅ Final Notes

* Your EC2 instance should have the correct **IAM role** attached.
* Your security group must allow **incoming traffic on port 5000**.
* Region and endpoint in `config.ini` must match your actual AWS region.


