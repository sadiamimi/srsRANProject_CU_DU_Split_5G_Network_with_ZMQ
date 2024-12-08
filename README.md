## 01. Basic Architecture

Referring resources I followed, \
https://docs.srsran.com/projects/project/en/latest/index.html and 
https://github.com/srsran/srsran_project
![image](5G_network_architecture.png)

  <ins>**Components:**</ins> 
  
  **5GC:** Open5GS.
  
  **NearRT-RIC:** ORAN SC platform with xApp integration.
  
  **CU/DU Split:** srsRAN or combined with OpenAirInterface.
  
  **srsUE:** [srsRAN 4G](https://github.com/srsran/srsRAN_4G) does include a prototype 5G UE (srsUE) which can be used for testing.

 We'll be demonstrating ZMQ based virtual radio setup.

 ## 02. VM Machine Readiness

<ins>**Machine Requirements:**</ins> 3 Ubuntu 22.04.1 LTS, 8cpu-8ram-128disk

<ins>**Machine Allocation:**</ins> 

**VM-1:** UE, Uplink Traffic

**VM-2:** DU, NearRT-RIC, xApp, Grafana

**VM-3:** 5GC, CU, Downlink Traffic

<ins>**Installing Docker for VM 2 & 3:**</ins>

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update


sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```

&nbsp;

<ins>**Installing mongodb (for 5GC) for VM 3 :**</ins>
```
#Download and add MongoDB GPG key:
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo tee /etc/apt/trusted.gpg.d/mongodb-org-6.0.gpg
#Add MongoDB repository to the list of sources:
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
#Update package list:
sudo apt update
#Install MongoDB:
sudo apt install -y mongodb-org
#Start the MongoDB service:
sudo systemctl start mongod
#Enable MongoDB to start on boot:
sudo systemctl enable mongod
#Check the status of MongoDB service:
sudo systemctl status mongod
```

&nbsp;

 ## 03. Running the Network
 The following order should be used when running the network:
1. Open5GS
2. NearRT-RIC
3. CU
4. DU
5. UE
6. Start Uplink and Downlink IP traffic (e.g., ping)
7. xApp
8. Grafana
