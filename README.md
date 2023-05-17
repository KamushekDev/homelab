# Raspberry pies

- Install x64 Lite Raspberry pi os with [imager](https://www.raspberrypi.com/software/) on **ssd** ~or sd card~
- Run pi with connected ssd, let it boot
- Connect ssd to pc 
  - open /boot/cmdline.txt
  - add `cgroup_memory=1 cgroup_enable=memory` to the end
- microk8s
  - installation (https://microk8s.io/docs/install-raspberry-pi)
    ```
    sudo apt install linux-modules-extra-raspi
    sudo apt update
    sudo apt install snapd
    sudo reboot
    sudo snap install microk8s --classic --channel=1.25
    sudo usermod -a -G microk8s pi
    sudo chown -f -R pi ~/.kube
    ```
  - connect nodes (https://microk8s.io/docs/clustering)  
    `microk8s add-node` on master node and follow instruction
- docker
  - installation
    ```
    sudo apt update
    sudo apt upgrade
    curl -sSL https://get.docker.com | sh
    sudo usermod -aG docker $USER
    # logout
    ```
  - magic :)  
    `sudo vim /etc/docker/daemon.json`  
    ```
    {
      "hosts": ["unix:///var/run/docker.sock", "tcp://<nodeip>:2375"]
    }
    ```  
    `sudo systemctl edit docker.service`  
    ```
    # some comments in here

    [Service]
    ExecStart=
    ExecStart=/usr/sbin/dockerd $DOCKER_OPTS

    ### Lines below this comment will be discarded

    # some comments in here
    ```
