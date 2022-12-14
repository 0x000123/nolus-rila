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

## Set up the minimum gas price and Peers/Seeds/Filter peers/MaxPeers
```python
PEERS="$(curl -s "https://raw.githubusercontent.com/Nolus-Protocol/nolus-networks/main/testnet/nolus-rila/persistent_peers.txt")"
```
Set them in the ~/.nolus/config/config.toml file:
```python
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" ~/.nolus/config/config.toml
```

### minimum gas price:
```python
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025unls\"/;" ~/.nolus/config/app.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.nolus/config/config.toml
external_address=$(wget -qO- eth0.me) 
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.nolus/config/config.toml
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.nolus/config/config.toml
sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 100/g' $HOME/.nolus/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 100/g' $HOME/.nolus/config/config.toml
```

### Pruning (optional)
```python
pruning="custom" && \
pruning_keep_recent="100" && \
pruning_keep_every="0" && \
pruning_interval="10" && \
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" ~/.nolus/config/app.toml && \
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" ~/.nolus/config/app.toml && \
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" ~/.nolus/config/app.toml && \
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" ~/.nolus/config/app.toml
```

### Indexer (optional)
```python
indexer="null" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nolus/config/config.toml
```

## Create a Service FILE
```python
sudo tee /etc/systemd/system/nolusd.service > /dev/null <<EOF
[Unit]
Description=nolusd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nolusd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Start node
```
sudo systemctl daemon-reload && sudo systemctl enable nolusd
sudo systemctl restart nolusd && sudo journalctl -u nolusd -f -o cat
```

# DONE
