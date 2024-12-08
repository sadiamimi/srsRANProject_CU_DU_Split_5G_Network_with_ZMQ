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

<ins>**Installing Docker:**</ins>

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

sudo docker run hello-world
```

&nbsp;
