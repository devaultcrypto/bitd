## What is BitD / BitDB?

BitD is a scraper for DeVault that fetches transaction data from the blockchain and stores it in a MongoDB database. BitD is a fork of the Fountainhead Cash implementation of Bitd (forked from _Unwriter/21 Century Motor Company). You need to install this to set up bitserve and run a bitdb node. 

### Hardware Requirements
If you intend to run a full un-pruned bitdb node, it is recommended that your server have at the very minimum **3 gigabytes of ram** or more available at all times, and that you have enough storage space to house the whole blockchain three times over. If this is not the case, you may look into changing the [core_from](https://github.com/fountainhead-cash/bitd/blob/master/.env.example#L15) enviorment variable to a more recent blockheight.

## Installation

### Prerequisite
First you need to do the following:
1. Install DeVault Core node software on your server
2. Install the latest versions of NPM and Nodejs on your server
2. Install MongoDB on your server

Before sync, set up your devault.conf file to meet the requirements set by bitd. On Ubuntu this can be found in $HOME/.devault/devault.conf

A sample configuration file can be found below:
```
# location to store blockchain and other data -- if you
# don't know what to do here, just remove the below line
datadir=/data/DeVault
dbcache=4000

# Must set txindex=1 so DeVault keeps the full index
txindex=1

# [rpc]
# Accept command line and JSON-RPC commands.
server=1
# Choose a strong password here
rpcuser=root
rpcpassword=devault

# If you want to allow remote JSON-RPC access
rpcallowip=0.0.0.0/0

# [wallet]
disablewallet=1

# [ZeroMQ]
# ZeroMQ messages power the realtime BitDB crawler
# so it's important to set the endpoint
zmqpubrawtx=tcp://127.0.0.1:28332
zmqpubhashblock=tcp://127.0.0.1:28332

# BitDB makes heavy use of JSON-RPC so it's set to a higher number
rpcworkqueue=512
```

### Setting up Bitd

Clone this repository:
```
git clone https://github.com/devaultcrypto/bitd.git && cd bitd
```

Install dependencies:
```
npm install
```

Configure bitd:
```
cp .env.example .env
$(EDITOR) .env
# note, you should only have to change rpc_user and rpc_pass normally
```

Start bitd:
```
npm start --max_old_space_size=4096
```

### Running as a daemon

Install PM2 using NPM
```
npm install pm2 -g
```

CD to install location and run bitd
```
pm2 start index.js --name "bitd" --node-args="--max_old_space_size=4096"
```

### Troubleshooting

#### View log

```
pm2 logs bitd
```

#### BitD crashing on bigger blocks

If it crashes, it's more than likely node is running out of memory, so try gradually increasing the max old space size.
```
pm2 start index.js --name "bitd" --node-args="--max_old_space_size=8192"
```

## Credits

2018 Unwriter

2018-current Fountainhead-Cash Developers
