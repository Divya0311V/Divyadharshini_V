<details>
  <summary>Day 0 - Tools Installation</summary>
   # **TOOLS VERIFICATION**
  

## Yosys
$ sudo apt-get update 
$ git clone https://github.com/YosysHQ/yosys.git 
$ cd yosys 
$ sudo apt install make (If make is not installed please install it)  
$ sudo apt-get install build-essential clang bison flex \ 
libreadline-dev gawk tcl-dev libffi-dev git \ 
graphviz xdot pkg-config python3 libboost-system-dev \ 
libboost-python-dev libboost-filesystem-dev zlib1g-dev 
$ make config-gcc 
$ make  
$ sudo make install 


![Image Alt](https://github.com/user-attachments/assets/0e5ccac1-ba08-4ab5-a829-23cc17e7640d)



## Iverilog 
sudo apt-get update 
sudo apt-get install iverilog 

![Image Alt](https://github.com/user-attachments/assets/11b9fd46-f292-4dab-b1b2-fdb0d20ed6c1)


## gtkwave 
sudo apt-get update 
sudo apt install gtkwave 


![Image Alt](https://github.com/user-attachments/assets/4496096e-f3f4-4d55-85d2-e2d9b77c6551)


## ngspice 
$ tar -zxvf ngspice-37.tar.gz 
$ cd ngspice-37 
$ mkdir release 
$ cd release 
$ ../configure  --with-x --with-readline=yes --disable-debug 
$ make 
$ sudo make install 

![Image Alt](https://github.com/user-attachments/assets/5a938b4e-5dcb-4438-a26f-bac60a79c201)



## magic 
$   sudo apt-get install m4 
$   sudo apt-get install tcsh 
$   sudo apt-get install csh 
$   sudo apt-get install libx11-dev 
$   sudo apt-get install tcl-dev tk-dev 
$   sudo apt-get install libcairo2-dev 
$   sudo apt-get install mesa-common-dev libglu1-mesa-dev 
$   sudo apt-get install libncurses-dev 
git clone https://github.com/RTimothyEdwards/magic 
cd magic 
./configure 
make 
make install 


![Image Alt](https://github.com/user-attachments/assets/6b3d42c1-e5b6-4a83-b9c5-cc042ca5565f)



## OpenLANE
sudo apt-get update 
sudo apt-get upgrade 
sudo apt install -y build-essential python3 python3-venv python3-pip make git 
sudo apt install apt-transport-https ca-certificates curl software-properties-common 
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o 
/usr/share/keyrings/docker-archive-keyring.gpg 
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] 
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee 
/etc/apt/sources.list.d/docker.list > /dev/null 
sudo apt update 
sudo apt install docker-ce docker-ce-cli containerd.io 
sudo docker run hello-world 
sudo groupadd docker 
sudo usermod -aG docker $USER 
sudo reboot  
# After reboot 
docker run hello-world 
Check dependencies  
git --version 
docker --version 
python3 --version 
python3 -m pip --version 
make --version 
python3 -m venv -h 

![Image Alt](https://github.com/user-attachments/assets/53597462-28f6-4794-b4fa-b382b1e8ca8d)



