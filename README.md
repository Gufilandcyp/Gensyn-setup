# Gensyn-setup
Gensyn/easy node setup
![image](https://github.com/user-attachments/assets/8ad5a694-e287-4d45-ba57-203f58a19714)

# Run RL Swarm (Testnet) Node
RL Swarm is a fully open-source framework developed by GensynAI for building reinforcement learning (RL) training swarms over the internet. This guide walks you through setting up an RL Swarm node and a web UI dashboard to monitor swarm activity.

## Hardware Requirements
Currently in the new recent update, Gensyn testnet is running and training the [reasoning-gym](https://github.com/open-thought/reasoning-gym/tree/main) swarm datasets on the Testnet. This swarm is supporting the current list of default models:
* Gensyn/Qwen2.5-0.5B-Instruct
* Qwen/Qwen3-0.6B
* nvidia/AceInstruct-1.5B
* dnotitia/Smoothie-Qwen3-1.7B
* Gensyn/Qwen2.5-1.5B-Instruct

* `GPU`: 
  * RTX 3090
  * RTX 4090
  * RTX 5090
  * A100
  * H100
  * `≥24GB vRAM` GPU is recommended, but Gensyn now supports `<24GB vRAM` GPUs too.

---

## 1) Install Dependencies
**1. Update System Packages**
```bash
apt update && apt upgrade -y
```
**2. Install General Utilities and Tools**
```bash
apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

**3. Install Python**
```bash
apt install python3 python3-pip python3-venv python3-dev -y
```

**4. Install Node**
```
apt update
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
apt install -y nodejs
node -v
npm install -g yarn
yarn -v
```

**5. Install Yarn**
```bash
curl -o- -L https://yarnpkg.com/install.sh | bash
```
```bash
export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"
```
```bash
source ~/.bashrc
```

---

## 2) Clone the Repository
```bash
git clone https://github.com/gensyn-ai/rl-swarm/
```

---

## 3) Run the swarm
* If you are an **existing user**, you must have your node's `swarm.pem` present in `rl-swarm` directory before starting the node, follow [Recover](#recover) instructions if need to recover `swarm.pem` file
* Use on of the `Bash` or `Docker` methods to run your node

### CLI Method (GPU)
1- Open a screen to run it in background
```bash
screen -S swarm
```
2- Get into the `rl-swarm` directory
```
cd rl-swarm
```
3- Install swarm
```bash
python3 -m venv .venv

source .venv/bin/activate
# if not worked, then:
. .venv/bin/activate

./run_rl_swarm.sh
```

---

## 4) Login
**1- You have to receive `Waiting for userData.json to be created...` in logs**

![image](https://github.com/user-attachments/assets/140f7d32-844f-4cf0-aac4-a91e9a14c1aa)

**2- Open login page in browser**
* **Local PC:** Open `http://localhost:3000/` in your browser
* **GPU Cloud & VPS Users: Tunnel to external URL:**
  * 1- Open a new terminal
  * 2- Install **localtunnel**:
    ```
    npm install -g localtunnel
    ```
  * 3- Get a password:
    ```
    curl https://loca.lt/mytunnelpassword
    ```
  * The password is actually your VPS IP
  * 4- Get URL
    ```
    lt --port 3000
    ```
  * Visit the prompted url, and enter your password to access Gensyn login page

**3- Login with your preferred method**

![image](https://github.com/user-attachments/assets/f33ea530-b15f-4af7-a317-93acd8618a9f)

* After login, your terminal starts installation.

**4- Answer prompts:**
* `Would you like to push models you train in the RL swarm to the Hugging Face Hub? [y/N]` >>> Press `N` to join testnet
  * `HuggingFace` needs `2GB` upload bandwidth for each model you train, you can press `Y`, and enter your access-token.
* `Enter the name of the model you want to use in huggingface repo/name format, or press [Enter] to use the default model.` >>> For default model, press `Enter`  or choose one of these (More model parameters (B) need more vRAM):
  * `Gensyn/Qwen2.5-0.5B-Instruct`
  * `Qwen/Qwen3-0.6B`
  * `nvidia/AceInstruct-1.5B`
  * `dnotitia/Smoothie-Qwen3-1.7B`
  * `Gensyn/Qwen2.5-1.5B-Instruct`

---

### Node Name
* Now your node started running, Find your name after word `Hello`, like mine is `whistling hulking armadillo` as in the image below (You can use `CTRL+SHIFT+F` to search Hello in terminal)

![image](https://github.com/user-attachments/assets/a1abdb1a-aa11-407f-8e5b-abe7d0a6b0f3)

---

### Screen commands
* Minimize: `CTRL` + `A` + `D`
* Return: `screen -r swarm`
* Stop and Kill: `screen -XS swarm quit`


---

# Run Multiple Nodes
- **Starting a New Node**: Launch a new node by connecting with the same email address on a new instance. Each new node generates a unique *Animal* name, *Peer ID* and creates a corresponding `swarm.pem` file as its identity.
- **Recovering an Animal Name**: To reuse an existing *Animal* name (e.g., for recovery), import the associated `swarm.pem` file into the new node.
- **Running Multiple Nodes**: You can run multiple nodes by either:
  - Installing the node on a new instance, or
  - Duplicating the repository with a new name and restarting the node within the duplicated repository and a new `swarm.pem` (it creates one when connecting the same email)
  - **Monitor Multiple Nodes**: Login via your email in the [dashboard](https://dashboard.gensyn.ai/) to see all instances of your nodes
 
  ![image](https://github.com/user-attachments/assets/e14e060e-c98a-4412-8850-6e2544fe2266)

---

## Backup
**You need to backup `swarm.pem`, if you want to recover your animal's name, animals are a subscribed to your email**
### `VPS`:
Connect your VPS using `Mobaxterm` client to be able to move files to your local system. Back up these files:**
* `/root/rl-swarm/swarm.pem`

### `WSL`:
Search `\\wsl.localhost` in your ***Windows Explorer*** to see your Ubuntu directory. Your main directories are as follows:
* If installed via a username: `\\wsl.localhost\Ubuntu\home\<your_username>`
* If installed via root: `\\wsl.localhost\Ubuntu\root`
* Look for `rl-swarm/swarm.pem`

### `GPU servers (e.g., Vast, Hyperbolic)`:
**1- Connect to your GPU server by entering this command in `Windows PowerShell` terminal**
```
sftp -P PORT ubuntu@xxxx.hyperbolic.xyz
```
* Replace `ubuntu@xxxx.hyperbolic.xyz` with your given GPU hostname
* Replace `PORT` with your server port (in your server ssh connection command)
* `ubuntu` is the user of my hyperbolic gpu, it can be ***anything else*** or it's `root` if you test it out for `vps`

Once connected, you’ll see the SFTP prompt:
```
sftp>
```

**2- Navigate to the Directory Containing the Files**
 * After connecting, you’ll start in your home directory on the server. Use the `cd` command to move to the directory of your files:
 ```
 cd /home/ubuntu/rl-swarm
 ```

**3- Download Files**
 * Use the `get` command to download the files to your `local system`. They’ll save to your current local directory unless you specify otherwise:
 ```
 get swarm.pem
 ```
* Downloaded file is in the main directory of your `Powershell` or `WSL` where you entered the sFTP command.
  * If entered sftp command in `Porwershell`, the `swarm.pem` file might be in `C:\Users\<pc-username>`.
* You can now type `exit` to close connection. The files are in the main directory of your `Powershell` or `WSL` where you entered the first SFTP command.

---

### Recover
If you need to upload files from your `local machine` to the `server`.
* `WSL` & `VPS`: Drag & Drop option.

**`GPU servers (.eg, Hyperbolic)`:**

1- Connect to your GPU server using sFTP

2- Upload Files Using the `put` Command:

In SFTP, the put command uploads files from your local machine to the server. 
```bash
put swarm.pem /home/ubuntu/rl-swarm/swarm.pem
```

---

# Node Health
### Official Dashboards
https://dashboard.gensyn.ai/

![image](https://github.com/user-attachments/assets/cd8e8cd3-f057-450a-b1a2-a90ca10aa3a6)

### Contract
Query your Node's peer ID for eoa address, rewards, wins, etc.
https://gensyn-testnet.explorer.alchemy.com/address/0xFaD7C5e93f28257429569B854151A1B8DCD404c2?tab=read_proxy

---

# Update Node
### 1- Stop Node
```console
# list screens
screen -ls

# kill swarm screens (replace screen-id)
screen -XS screen-id quit

# You can kill by name
screen -XS swarm quit
```
