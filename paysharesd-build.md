# Building Paysharesd

Installation of paysharesd on Ubuntu 14.04 is relatively straightforward with administrator privileges:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git autoconf autogen automake build-essential software-properties-common libboost-all-dev libcurl4-openssl-dev libdb-dev libdb++-dev libgmp3-dev libminiupnpc-dev libmpfr-dev libssl-dev libcurl4-openssl-dev libjansson-dev pax-utils
sudo apt-get install git scons ctags pkg-config protobuf-compiler libprotobuf-dev libssl-dev python-software-properties libboost1.55-all-dev g++ make
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 40 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8
sudo update-alternatives --config gcc
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
tar xzf LATEST.tar.gz
cd libsodium-0.6.1/
./configure
make && make check && sudo make install
cd ..
git clone https://github.com/payshares/paysharesd.git
cd paysharesd/
scons
```

After compilation, paysharesd will be available within the 'build' directory.  paysharesd.cfg should be placed in the ~/.config/payshares directory.  Create the following directories:

```
(if not exists):
mkdir ~/.config

mkdir ~/.config/payshares
mkdir ~/.config/payshares/db
```

The published documentation will be updated to reflect the initial validating node as referenced in the following example paysharesd.cfg:

```
# Allow other peers to connect to this server.
#
[peer_ip]
0.0.0.0

[peer_port]
52011

# Allow untrusted clients to connect to this server.
#
[websocket_public_ip]
0.0.0.0

[websocket_public_port]
5016

# Provide trusted websocket ADMIN access to the localhost.
#
[websocket_ip]
127.0.0.1

[websocket_port]
6016

# Provide trusted json-rpc ADMIN access to the localhost.
#
[rpc_ip]
127.0.0.1

[rpc_port]
5015

[rpc_allow_remote]
0

[node_size]
medium

# This is primary persistent datastore for paysharesd.  This includes transaction
# metadata, account states, and ledger headers.  
[node_db]
type=RocksDB
path=<userpath>/.config/payshares/db/rocksdb
open_files=2000
filter_bits=12
cache_mb=256
file_size_mb=8
file_size_mult=2

[database_path]
<userpath>/.config/payshares/db

# This needs to be an absolute directory reference, not a relative one.
# Modify this value as required.
[debug_logfile]
<userpath>/.config/payshares/debug.log

[sntp_servers]
time.windows.com
time.apple.com
time.nist.gov
pool.ntp.org

# Where to find some other servers speaking the Payshares protocol.
#
[ips]
one.validator.payshares.co

[validators]
naKF9vP6PmUDudBVkXHfr3cRScQideaMsFa9crvntk7x9VYPUTr SDF1

# Ditto.
[validation_quorum]
1

[consensus_threshold]
-1

# Turn down default logging to save disk space in the long run.
# Valid values here are trace, debug, info, warning, error, and fatal
[rpc_startup]
{ "command": "log_level", "severity": "warning" }

# Configure SSL for WebSockets.  Not enabled by default because not everybody
# has an SSL cert on their server, but if you uncomment the following lines and
# set the path to the SSL certificate and private key the WebSockets protocol
# will be protected by SSL/TLS.
#[websocket_secure]
#1

#[websocket_ssl_cert]
#/etc/ssl/certs/server.crt

#[websocket_ssl_key]
#/etc/ssl/private/server.key

# Defaults to 0 ("no") so that you can use self-signed SSL certificates for
# development, or internally.
#[ssl_verify]
#0
```
