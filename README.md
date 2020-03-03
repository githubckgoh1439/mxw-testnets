
# Install Maxonrow Local Network 
This guide will go through the steps required to get a Full Node up and running on MAXONROW's local network.<br/>
It also include the steps for running a Single Node that connect the Full Node.

** These steps are to be run on an **Ubuntu 18.04** or higher version and logged in as **root** user.


## From Source

You'll need `go` [installed](https://golang.org/doc/install) and the required
environment variables set, which can be done with the following commands:

```sh
# Set the GOPATH variables
echo export GOPATH=\"\$HOME/go\" >> ~/.bash_profile

# Set the PATH variables along with GOPATH
echo export PATH=\"\$PATH:\$GOPATH/bin\" >> ~/.bash_profile

# Set the GO111MODULE variables
echo export GO111MODULE=on >> ~/.bash_profile
```

### Get Source Code

```sh
# Make a new directory for this Maxonrow Project
mkdir -p $GOPATH/src/github.com/maxonrow/

# Go to maxonrow directory
cd $GOPATH/src/github.com/maxonrow/

# Clone maxonrow-go source 
git clone https://github.com/maxonrow/maxonrow-go.git

# Go to maxonrow-go directory
cd maxonrow-go

# Checkout a particular branch.
git checkout master 
```

### Get Tools & Dependencies, Compile, Run

```sh

# Get all the dependecies and build project the binary
make all

# Initial the blockchain network with a valid chain-id, 
# which will generate the config, genesis and account in respective folder
mxwd init --home ~/.mxw_fullnode

# Generate validator transcation, 
# which will create the gentx folder with gentx transcation of validator account-1
mxwd gentx --name acc-1 --home ~/.mxw_fullnode

# Start the Full node
mxwd start --home ~/.mxw_fullnode

```

Open browser and enter http://localhost:26657/<br/><br/>
** This means that Full Node already up and running !!!


### Next step is to setup a Single Node that will synch with the Full Node.

```sh

# Start the node by connecting to Full node via p2p.seeds 
# which the value come from memo section of gentx.json inside Full node  
mxwd start --home ~/.mxw_node --log_level info --p2p.seeds 9e328b68690ad433ebcd51e83c38df12530716d3@192.168.20.42:26656

```

```
** Hints while setup Single node

Problem-1 : Couldn't read GenesisDoc file, no such file or directory under this new node
Solution : Replace with genesis.json that come from Full node

Problem-2 : Failed to listen on 127.0.0.1:26657, address already in used by Full node
Solution : Change this port-number where laddr = "tcp://127.0.0.1:36657" under section of rpc in the config.toml 

Problem-3 : ERROR: listen tcp 0.0.0.0:26656: bind: address already in used by Full node
Solution : Change this port-number where laddr = "tcp://127.0.0.1:36657" under section of p2p in the config.toml 


```


Open browser and enter http://localhost:36657/<br/><br/>
** This means that new node already running and try to synch with current Full node !!!
