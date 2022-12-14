# Run a Full Node

---

# 💻⚙ Hardware Requirements

The recommended hardware to run a Nolus node will vary depending on the node's use case and desired functionalities. For example, if the node acts as an archive, meaning that it stores all the historical data of the blockchain dating back to the genesis block, it would naturally require a lot of storage (disk space). The recommended by us minimum requirements to run a full node are the following:

- 2+ vCPU
- 4+ GB RAM
- 120+ GB SSD

# 👨‍🏭 **Prerequisites**

Golang v1.16.1 - go1.18.1 linux/amd64
Linux users
```python
sudo apt-get install -y build-essential
```

# 🏗 Build Nolus Core

### 1. Use Git to Clone the Nolus Core Repository

```bash
git clone https://github.com/Nolus-Protocol/nolus-core
```

### 2. Make Sure That You Are in the “Core” Directory
```
cd nolus-core
```

### 3. Check Out the Main Branch Containing the Latest Stable Release

```bash
git checkout [latest version]
```

*ex., git checkout v0.1.39*

### 4. Compile the Nolus Core and Install It

```bash
make install
```

### 5. Verify That Nolus Core Is Installed Correctly by Checking the Version
```
nolusd version --long
```

# 🔨 Configuration and Execution

### 1. Initialize a Nolus Node
```python
nolusd config chain-id nolus-rila
nolusd init [custom_moniker] --chain-id nolus-rila
```

2. Obtain the Genesis File and Set Persistent Peers
```python
wget https://raw.githubusercontent.com/Nolus-Protocol/nolus-networks/main/testnet/nolus-rila/genesis.json
```

```python
mv ./genesis.json ~/.nolus/config/genesis.json
```

### Peers, Pruning, Indexer
