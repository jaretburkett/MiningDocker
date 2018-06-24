# Mining Docker

###### Various Docker images for crypto mining on NVIDIA GPUs

We currently only have a few miners, but I will add more as I need them or they are requested. 
The goal of this project is to make setting up and updating miners on Linux systems easier. 
I was tired of installing dependencies, updating/building the miners for each rig, etc. This is easier.

## Current Miners

| Miner | Version | Algo | Docs | Pull Command |
| ----- | :-----: | ---- | ---- | ---------- |
| [ccminer](https://github.com/tpruvot/ccminer) | 2.3 | multi | [readme](https://github.com/tpruvot/ccminer/blob/linux/README.txt)| `docker pull miningdocker/ccminer:latest` |
| [dtsm-zm](https://bitcointalk.org/index.php?topic=2021765.0) | 0.6.1 | equihash | [bitcoin talk](https://bitcointalk.org/index.php?topic=2021765.0)| `docker pull miningdocker/dtsm-zm:latest` |
| [ethminer](https://github.com/ethereum-mining/ethminer) | 0.14.0 | daggerhash | [github](https://github.com/ethereum-mining/ethminer)| `docker pull miningdocker/ethminer:latest` |


## Usage
I recommend using Ubuntu 16.04 for running these containers, but any Linux variant should work fine.

#### Setup

First install [Nvidia Drivers](http://www.nvidia.com/Download/index.aspx), 
[Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/), 
[Nvidia Docker](https://github.com/NVIDIA/nvidia-docker) 

#### Pull / Build Mining Containers

You can either [build the containers](#building) locally or pull them from docker hub. You only need to
build/pull the containers for the miners you want. See the **Pull Command** in the [Current Miners](#current-miners)
section


#### Create an alias

To create an alias, to make it easier to run these images, insert the following line at the end of
your `~/.bash_aliases` file. Create it if it does not exist.

```bash
alias miningdocker='function miningdockerfn() { nvidia-docker run --rm -it miningdocker/${@:1:$#}; }; miningdockerfn'
```

You will need to reactivate your profile after inserting it, so run
```bash
source ~/.bashrc
```

From now on, you can simply run 
```bash
miningdocker <minername> [options]
```

You **do not** need to create a seperate alias for each image, the above alias will select the appropriate image for you.

#### Update

To update your container, simply shut it down and pull the **latest** image from the docker hub

```bash
docker pull miningdocker/<minername>:latest
```

## Examples

These examples assume you have build the images and setup the alias
```bash
# ccminer
miningdocker ccminer -a lyra2v2 -o stratum+tcp://lyra2rev2.usa.nicehash.com:3347 -u <btcaddress>.<workername> -p x

# dtsm-zm
miningdocker dtsm-zm --server equihash.usa.nicehash.com --port 3357 --user <btcaddress>.<workername>

# ethminer (note: --cuda flag is not needed, it is in the docker file)
miningdocker ethminer -P stratum2+tcp://<btcaddress>.<workername>@daggerhashimoto.usa.nicehash.com:3353
```


## Building

The built images are on docker hub for ease of use. However, when dealing with crypto
you always have to be careful. I recommend always building crypto based dockers (all docker containers really)
from the docker files so you can review what is happening. To build these docker containers. Run the build
commands from the project source directory for any miner you would like.

```bash
nvidia-docker build -t miningdocker/<minername> -f <dockerfile> .
```

Example: For ccminer

```bash
nvidia-docker build -t miningdocker/ccminer -f ccminer.docker .
```


## Donate

Donations are never expected, but always appreciated

| Coin | Address |
| ---- | ------- |
| BTC | **1HCHFfJqUi4LSVaJE7AVyRAb8Gd4cqET7D** |
| BCH | **qrssq66f767flng9eur9w8cmp7kze9pvy5j3dd5qqq** |
| DASH | **XsQipH3bGckBVyCHNYaDec1B6aD3NvCRLN**|
| ETH | **0x277f3df4db54463481c25476b8fc0cb327ec29ca**|
| DOGE | **DGoCk6S6TgRG9zgMFmAV2kYBNShMN83LwJ**|
| LTC | **LhYZBybhqwYGrLtprcLy7fvVBgzmqKNryJ**|




