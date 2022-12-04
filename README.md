
![Logo](https://ka-tet.s3.amazonaws.com/logo-black.png)

***"A group of people bound by faith or destiny"***

## Repeatable, immutable, and scalable Security Research w/ Docker


## Acknowledgements

 I pulled alot of inspiration from this awesome project and can only hope my project is as awesome as this one is.
 
- [Exegol - Fully featured and community-driven hacking environment](https://github.com/ThePorgs/Exegol)

## Fully Tested on: Debian 11, Arch Linux 5.17.8-zen

Docker is a life saver for reproducing a consistent environment for various types of assessments. Instead of wasting time configuring a Kali Linux virtual machine, this Docker image provides a repeatable environment for any type of engagement. 

## Features

- Command history logging
- Terminal Output logging using script
- Persistance volume/workspace


Custom zsh aliases included: 

| alias      | command                                                                                              |
|------------|------------------------------------------------------------------------------------------------------|
| http       | 'sudo python3 -m http.server 80'                                                                     |
| reload     | '. ~/.zshrc'                                                                                         |
| update     | 'sudo apt-get update'                                                                                |
| t          | 'tmux -f ~/.tmux.conf'                                                                               |
| zcopy      | 'cp .zshrc .zlogin && cp .zlogin .zprofile'                                                          |
| i          | 'sudo apt-get install'                                                                               |
| public     | 'curl wtfismyip.com/text'                                                                                   |
| speedtest  | 'curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python3 -'     |
| scan       | 'nmap -sC -sV -p- -oA $NAME'                                                                                |
| ffuf-quick | 'ffuf -u $URL -w /usr/share/seclists/Discovery/Web-Content/common.txt' -o $NAME-results.txt -mc 200 -fs 0' |
| portscan   | 'cp /root/nishang/Scan/Invoke-PortScan.ps1 .'                                                         |
| turbo      | 'python3 ~/tools/Turbolist3r/turbolist3r.py'                                                          |
| cloudscrap | 'python3 ~/tools/CloudScraper/CloudScraper.py'                                                        |

## Usage/Examples

Initial Set up and testing:

```bash

$ git clone https://github.com/alexrf45/Ka-tet.git

$ cd Ka-tet

$ cat functions >> ~/.zshrc

$ . ~/.zshrc

$ export NAME=test

$ kali

```

Pre-engagement:

```bash

$ export NAME="engagement-20xx-xx-xx-company.com"

$ mkdir $NAME && cd $NAME

```

Engagement: 

```bash
$ kali

Output:

[ Sun Dec 04 2022  2:21PM ]  [ root@dev-local:/engagement-20xx-xx-xx-company.com ]
 $ 

$ logfile $NAME

$ cd /$NAME
```
Customization: 

- You can build the image locally: 
```bash

$ cd Ka-tet

$ export IMAGE_NAME=<desired image name and tag>

$ docker build -t $IMAGE_NAME .

```

- Feel free to edit the `zshrc` and add command examples in the `kali_history` based on desired configuration

Container Functions for your .zshrc or .bashrc: 

```bash

function kali() {
	dirname=${PWD##*/}
	if [ ! -d `pwd`/.kali-logs ];
	then
   	 	mkdir .kali-logs \
   	 	&& docker run --name $NAME --rm -it \
   	 	--net=host --entrypoint=/bin/zsh \
		--cap-add=NET_ADMIN \
   	 	-v $HOME/.Xauthority:/root/.Xauthority:ro -v /tmp/.X11-unix:/tmp/.X11-unix \
    	-e DISPLAY=$DISPLAY \
		-e NAME=$NAME \
   	 	-v `pwd`/.kali-logs:/root/.logs:rw -v `pwd`:/${dirname} \
   	 	-w /${dirname} fonalex45/project-repo-1:kali-full
	else
		docker run --name $NAME --rm -it \
		--net=host --entrypoint=/bin/zsh \
		--cap-add=NET_ADMIN \
		-v $HOME/.Xauthority:/root/.Xauthority:ro -v /tmp/.X11-unix:/tmp/.X11-unix \
    	-e DISPLAY=$DISPLAY \
		-e NAME=$NAME \
		-v `pwd`/.kali-logs:/root/.logs:rw -v `pwd`:/${dirname} \
		-w /${dirname} fonalex45/project-repo-1:kali-full
	fi
}

function enter-kali() {
	docker exec -it $NAME /bin/zsh
}

```