# Problem Statement

## *The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:*

## a. *Create a docker network named as ecommerce on App Server 3 in Stratos DC.*

## b. *Configure it to use macvlan drivers.*

## c. *Set it to use subnet 172.28.0.0/24 and iprange 172.28.0.0/24.*

# Solution:

## Docker network create

```bash
docker network create ecommerce -d macvlan --subnet=172.28.0.0/24 --ip-range=172.28.0.0/24
```
## Docker Network inspect ecommerce: 
```bash
[root@stapp03 banner]# docker network inspect ecommerce
[
    {
        "Name": "ecommerce",
        "Id": "394923b894e14c1705164b0b8a203d3fc47f5f6457168a7edcd668a705006659",
        "Created": "2024-07-10T13:11:58.623314771Z",
        "Scope": "local",
        "Driver": "macvlan",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.28.0.0/24",
                    "IPRange": "172.28.0.0/24"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
```
