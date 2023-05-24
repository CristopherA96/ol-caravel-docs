# OpenLane and Caravel Setup
---


## OpenLane

!!! note
    All these steps are to be follow in a **debian-based** distro, such as Ubuntu, Debian, Pop OS!, etc.

---

To install OpenLane properly we need to follow some steps:

### Step 1: Install dependencies

```yaml

$ sudo apt update                                                                   ; # Update your system
$ sudo apt full-upgrade                                                             ; # Upgrade all your packages
$ sudo apt install -y build-essential python3 python3-venv python3-pip make git     ; # Install some dependencies

```

OpenLane runs into a docker container, so we need to install it.

```yaml
$ sudo apt remove --purge docker docker-engine docker.io containerd runc    ; # Remove old docker versions
$ sudo apt install ca-certificates curl gnupg lsb-release                   ; # Dependencies for docker
$ sudo mkdir -p /etc/apt/keyrings                                           ; # Create folder to add a new keyring if you do not have that folder
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg          ; # Getting the docker GPG key
$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null                  ; # Setup repository

$ sudo apt update                                                           ; # Refresh your system
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-buildx-plugin  ; # Install docker packages

```

Setting up docker rootless mode.

```yaml

$ sudo groupadd docker                        ; # Create docker group if needed
$ sudo usermod -aG docker $USER               ; # Add your user to the docker group
$ newgrp docker                               ; # Activate changes to docker group
$ docker run hello-world                      ; # Run docker image to test

```

### Step 2: Install OpenLane

```yaml

$ mkdir eda_tools
$ cd eda_tools

$ git clone --depth 1 https://github.com/The-OpenROAD-Project/OpenLane.git      ; # Clone OpenLane repository
$ cd OpenLane/
$ echo "export OPENLANE_ROOT=\"/home/\$USER/eda_tools/OpenLane\"" >> ~/.bashrc  ; # Add OPENLANE_ROOT path
$ echo "export PDK_ROOT=\"\$OPENLANE_ROOT/pdks\"" >> ~/.bashrc                  ; # Add PDK_ROOT path
$ echo "export PDK_PATH=\"\$PDK_ROOT/sky130A\"" >> ~/.bashrc                    ; # Add PDK_PATH path

$ source ~/.bashrc                                                              ; # Refresh env variables
$ make                                                ; # Install OpenLane
$ make test                                           ; # Run OpenLane test

```


## Caravel
---

### Step 1: Install caravel SoC

To install caravel SoC framework, we should have already installed python3.6+ with pip and docker, as well.

```yaml
; # Create a template going to the next URL

https://github.com/efabless/caravel_user_project/generate

$ cd eda_tools

$ git clone <your github repo>
$ cd <project name>
$ mkdir dependencies

; # Before adding this new paths, comment the ones about OpenLane

$ echo "export OPENLANE_ROOT=\"/home/\$USER/eda_tools/OpenLane\"" >> ~/.bashrc
$ echo "export PDK_ROOT=\"\$OPENLANE_ROOT/pdks\"" >> ~/.bashrc              
$ echo "export PDK_PATH=\"\$PDK_ROOT/sky130A\"" >> ~/.bashrc         

$ echo "alias mup=\"make user_proj_example\"" >> ~/.bashrc
$ echo "alias mupw=\"make user_project_wrapper\"" >> ~/.bashrc

; # Refresh environment variables
$ source ~/.bashrc                                              

; # Install Caravel

$ make setup

```

### Simulating in Caravel
---

We can run simulation of our design as well.

```yaml

$ time make verify-<your-tb>-rtl                ; # Running simulation using RTL

$ time verify-<your-tb>-gl                      ; # Running simulation using netlist

```

!!! info
    To generate a template, you shold have a GitHub account already created.












