
## Problem Statement

The Nautilus DevOps team needs to establish several Docker environments for different applications. One specific task involves setting up a Docker network on App Server 3 in the Stratos Datacenter. The network should be named `ecommerce` and configured with specific settings. The requirements are:

- **Create a Docker network named `ecommerce`** on App Server 3.
- **Configure the network to use the `macvlan` driver.**
- **Set the network to use the subnet `172.28.0.0/24`** and IP range `172.28.0.0/24`.

## Solution

Follow these steps to create and configure the Docker network as specified:

1. **Create the Docker Network**

   Use the `docker network create` command to establish a new network with the name `ecommerce`. Configure it to use the `macvlan` driver, and specify the desired subnet and IP range.

   ```bash
   docker network create ecommerce -d macvlan --subnet=172.28.0.0/24 --ip-range=172.28.0.0/24
   ```

   In this command:
   - `ecommerce` is the name of the network.
   - `-d macvlan` specifies the use of the `macvlan` driver.
   - `--subnet=172.28.0.0/24` sets the network's subnet.
   - `--ip-range=172.28.0.0/24` defines the IP address range within the subnet.

2. **Verify the Network Configuration**

   To confirm that the network has been created and configured correctly, use the `docker network inspect` command. This command provides detailed information about the `ecommerce` network.

   ```bash
   docker network inspect ecommerce
   ```

   Expected output:

   ```json
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
   ]
   ```

   This output confirms that the `ecommerce` network has been created with the correct settings:
   - The network driver is `macvlan`.
   - The subnet and IP range are correctly configured.
