# Building environment (for Ubuntu)

## Docker

    sudo apt install docker docker.io

    sudo usermod -aG docker $USER
    docker ps  # Check that it does not output "Permission denied"


## Containerlab

[Installation guide](https://containerlab.dev/install/)

    echo "deb [trusted=yes] https://apt.fury.io/netdevops/ /" | sudo tee -a /etc/apt/sources.list.d/netdevops.list
    sudo apt update && sudo apt install containerlab


## dot2net

[Installation guide](https://github.com/cpflat/dot2net/readme.md)

    git clone https://github.com/cpflat/dot2net.git
    cd dot2net
    docker run --rm -i -t -v $PWD:/v -w /v golang:1.18 go build
    mv dot2net /usr/local/bin/

