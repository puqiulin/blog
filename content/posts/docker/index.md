---
title: "Docker proxy"
date: "2024-02-01"
description: "When using Docker, due to network reasons, it is usually necessary to use an HTTP/HTTPS proxy to speed up image pulling, construction and use"
categories: ["docker"]
---
# Set up proxy for dockerd
The "docker pull" command is executed by the dockerd daemon. The dockerd daemon is managed by systemd. Therefore, if you need to use the HTTP/HTTPS proxy when executing the "docker pull" command, you need to configure it through systemd.


## 1.Create configuration folder for dockerd.
```shell
sudo mkdir -p /etc/systemd/system/docker.service.d
```

## 2.Create http-proxy.conf file under the folder, add follow content
```shell
sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf

[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080/"
Environment="HTTPS_PROXY=http://proxy.example.com:8080/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

## 3.Restart docker
```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# Set up proxy for docker container
During the container running phase, if you need to use HTTP/HTTPS proxy, you can change the docker client configuration or specify environment variables.

## Change docker client configuration

### 1.Create or update follow file
```shell
sudo vim ~/.docker/config.json
```

### 2.Add follow content
```shell
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://proxy.example.com:8080/",
     "httpsProxy": "http://proxy.example.com:8080/",
     "noProxy": "localhost,127.0.0.1,.example.com"
   }
 }
}
```

## Change environment variables
| Environment | In docker CMD |
| - | -|
| HTTP_PROXY  | --env HTTP_PROXY="http://proxy.example.com:8080/" |
| HTTPS_PROXY | --env HTTPS_PROXY="http://proxy.example.com:8080/" |
| NO_PROXY | --env NO_PROXY="localhost,127.0.0.1,.example.com" |

# Set up proxy for docker build
During the container build phase, if you need to use an HTTP/HTTPS proxy, you can specify the environment variable of "docker build" or specify the environment variable in the Dockerfile.

## Use "--build-arg" for "docker build" variable
```shell
docker build \
    --build-arg "HTTP_PROXY=http://proxy.example.com:8080/" \
    --build-arg "HTTPS_PROXY=http://proxy.example.com:8080/" \
    --build-arg "NO_PROXY=localhost,127.0.0.1,.example.com" .
```

## Add environment variable in Dockerfile
| Environment | In Dockerfile |
| - | -|
| HTTP_PROXY  | ENV HTTP_PROXY="http://proxy.example.com:8080/" |
| HTTPS_PROXY | ENV HTTPS_PROXY="http://proxy.example.com:8080/" |
| NO_PROXY | ENV NO_PROXY="localhost,127.0.0.1,.example.com" |