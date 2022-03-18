# NGINX Instance Manager Demo

## Requirements:
Requires Ubuntu, tested on 20.04

## Installation
Clone this repository:

```bash
$ git clone https://github.com/f5dsse/nim-demo.git
```

Get NIM trial license here (or from your F5 rep): https://www.nginx.com/products/nginx-instance-manager/ 

Download the license bundle including the files:
- nginx-instance-manager-eval.crt
- nginx-instance-manager-eval.key
- nginx-instance-manager-eval.lic

Make a license subfolder in the repo directory: 
```bash
$ cd nim-demo
$ mkdir license
```

Copy `nginx-instance-manager-eval.crt` and `nginx-instance-manager-eval.key` to the `license` subfolder of the repository you cloned and rename them to `nginx-repo.crt` and `nginx-repo.key` respectively. 

Directory structure should look like this:
```
├── license
│   ├── nginx-repo.crt
│   └── nginx-repo.key
└── scripts
    ├── nim
    ├── oss
    ├── oss-agent
    ├── plus
    └── plus-agent
```
Install NGINX Instance Manager via the script

```bash
$ scripts/nim
```

Copy the admin password for the NGINX Manager UI from the script output, and go to `https://<instance-ip>/`, then log in.

Upload the license (`nginx-instance-manager-eval.lic`) to Settings -> Licenses

Congratulations, NGINX Instance Manager is set up.

## Testing

Scripts are provided to quickly spin up managed and unmanaged instances for testing using LXC: 

First init LXD (just press enter through all the prompts):

```bash
lxd init
```

Start an unmanaged OSS instance:

```bash
scripts/oss
```

Start a managed OSS instance:

```bash
scripts/oss-agent
```

Start a managed OSS instance and add it to an instance group:

```bash
scripts/oss-agent <instance group name>
```

Start an unmanaged Plus instance:

```bash
scripts/plus
```

Start a managed Plus instance:

```bash
scripts/plus-agent
```

Start a managed Plus instance and add it to an instance group:

```bash
scripts/plus-agent <instance group name>
```

To scan for instances created via these scripts, find the CIDR of the container bridge network: 

```
lxc network info lxdbr0 | grep inet | awk '{print $2}' | head -1
```

example output: `10.176.104.1`

In NGINX Instance Manager, click "Instance Manager" -> Scan

In the CIDR field, put the /24 network that matches your lxdbr0 gateway IP, e.g. `10.176.104.0/24`
