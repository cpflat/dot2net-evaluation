# 環境構築例 (Ubuntuの場合)

## Dockerの環境構築

    sudo apt install docker docker.io

    sudo usermod -aG docker $USER
    docker ps  # "Permission denied"が出ないことを確認


## Containerlabの環境構築

[インストールガイド](https://containerlab.dev/install/)

    echo "deb [trusted=yes] https://apt.fury.io/netdevops/ /" | sudo tee -a /etc/apt/sources.list.d/netdevops.list
    sudo apt update && sudo apt install containerlab


## dot2netの環境構築

[インストールガイド](https://github.com/cpflat/dot2net/readme.md)

    git clone https://github.com/cpflat/dot2net.git
    cd dot2net
    docker run --rm -i -t -v $PWD:/v -w /v golang:1.18 go build
    mv dot2net /usr/local/bin/

