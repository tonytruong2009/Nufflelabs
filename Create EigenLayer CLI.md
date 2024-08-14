# EigenLayer CLI installation and registration

## System Requirements
| Category | Requirements |
| ------------ | ------------ |
| CPU | x86_64 (Intel, AMD) processor with at least 8 physical cores |
| CPU | CMPXCHG16B, POPCNT, SSE4.1, SSE4.2, AVX |
| RAM | 32GB DDR4 |
| Storage | 1.5TB SSD (NVMe SSD is recommended) |
| Operating System	 | 	Linux (tested on Ubuntu 22.04 LTS) |

### Verify CPU feature support by running the following command on Linux:
```bash
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```


### 1. Install Docker & Docker compose
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
docker version
```
```bash
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose --version
```


### 2. Install EigenLayer CLI
```bash
curl -sSfL https://raw.githubusercontent.com/layr-labs/eigenlayer-cli/master/scripts/install.sh | sh -s
export PATH=$PATH:~/bin
eigenlayer --version
```


### 3. Clone NFFL repo
```bash
git clone https://github.com/NethermindEth/near-sffl.git
cd near-sffl/setup/plugin
cp .env.example .env
```


### 4. Create/Import Eigenlayer wallet
```bash
eigenlayer operator keys create --key-type ecdsa "wallet_name" (REMEMBER CHANGE THE WALLET NAME)
```
Set password & save your private key (DON'T FORGET SAVE THIS)

### Import an old key (OPTIONAL)
```bash
eigenlayer operator keys import --key-type ecdsa "wallet_name" PRIVATEKEY (CHANGE PRIVATE BY UR PRIVATE KEY)
```


### 5. Fund your Eigen wallet

You’ll need at least `1 Holesky ETH` 
to cover the gas cost of the operator registration. Make sure to send at least 1 ETH to your Eigenlayer operator’s address

Link faucet Holesky ETH:

https://cloud.google.com/application/web3/faucet/ethereum/holesky

https://holesky-faucet.pk910.de/

 
### 6. Config & register operator
```bash
eigenlayer operator config create
```
- Enter your operator address: `your Eigenlayer address`

- Enter your earnings address (default to your operator address): `your Eigenlayer address`
  
- Enter your ETH rpc url: `https://ethereum-holesky-rpc.publicnode.com`
  
- Select your network: `holesky`
   
- Select your signer type: `local_keystore`
  
- Enter your ecdsa key path: `/root/.eigenlayer/operator_keys/joseph-test1.ecdsa.key.json`


### 7. Edit metadata.json
```bash
nano ~/near-sffl/setup/plugin/metadata.json
```
Sample:
```bash
{
  "name": "roscuong",
  "website": "https://www.josephtran.xyz",
  "description": "Hard Worker - Learning 4ever",
  "logo": "https://raw.githubusercontent.com/roscuong/Nuffle/main/roscuong0909.png",
  "twitter": "https://x.com/roscuong"
}
```
### IMPORTANT: Logo support .png only and less than 1Mb
- Create a Public repositry in github
- Upload `logo.png`
   and `metadata.json`
  there.


### 8. Edit operator.yaml
```bash
nano ~/near-sffl/setup/plugin/operator.yaml
```
Set URL metadatar with raw file link:

- metadata_url:
  `https://raw.githubusercontent.com/roscuong/Nuffle/main/metadata.json`
  ![image](https://github.com/user-attachments/assets/197cbfec-4717-4396-a6ae-a8c1ce453bff)


### 9. Register Eigenlayer Operator (holesky)
```bash
eigenlayer operator register operator.yaml
```
![image](https://github.com/user-attachments/assets/cf18089d-4425-4497-b060-0b7891fe244a)

You Operator link:
https://holesky.eigenlayer.xyz/operator/0x0d11e77b16cee6477b21e4e38249a253383fbee8

**Check status:**
```bash
eigenlayer operator status operator.yaml
```

![image](https://github.com/user-attachments/assets/bda55c7e-c53f-4217-aec8-9bedd315f437)


### 10. Setup & Register NFFL Operator

**At this initial testnet stage, operators need to be whitelisted. If you are interested and have not already been whitelisted, please contact the NFFL team!**

 
