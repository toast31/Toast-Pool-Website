Prereqs.sh:

mkdir "$HOME/tmp";cd "$HOME/tmp"
curl -sS -o prereqs.sh https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/prereqs.sh
chmod 755 prereqs.sh
./prereqs.sh
. "${HOME}/.bashrc"

Pool ID: 4ca9249724d0d5229e0538163933f31986599b4d378c7eb26ca2e88b


~~Mainnet config/topology files:~~

~~curl -sL -o $CNODE_HOME/files/byron-genesis.json https://hydra.iohk.io/job/Cardano/iohk-nix/cardano-deployment/latest-finished/download/1/mainnet-byron-genesis.json~~
~~curl -sL -o $CNODE_HOME/files/genesis.json https://hydra.iohk.io/job/Cardano/iohk-nix/cardano-deployment/latest-finished/download/1/mainnet-shelley-genesis.json~~
~~curl -sL -o $CNODE_HOME/files/topology.json https://hydra.iohk.io/job/Cardano/iohk-nix/cardano-deployment/latest-finished/download/1/mainnet-topology.json~~
~~curl -sL -o $CNODE_HOME/files/config.json https://raw.githubusercontent.com/cardano-community/guild-operators/master/files/ptn0-mainnet.json~~

Building cardano-node:

cd ~/git
git clone https://github.com/input-output-hk/cardano-node
cd cardano-node

git fetch --tags --all
git checkout 1.18.0
git pull origin master

echo -e "package cardano-crypto-praos\n  flags: -external-libsodium-vrf" > cabal.project.local
$CNODE_HOME/scripts/cabal-build-all.sh

Sync the node:

cd $CNODE_HOME/scripts
tmux
nano cnode.sh




--shelley-kes-key /opt/cardano/cnode/priv/pool/Kobe/hot.skey
--shelley-vrf-key /opt/cardano/cnode/priv/pool/Kobe/vrf.skey
--shelley-operational-certificate /opt/cardano/cnode/priv/pool/Kobe/op.cert











FAQ:

1) CNTools - WARN: Shelley era not reached!
 - echo "208" > $CNODE_HOME/db/shelley_trans_epoch

2) Metadata error on pooltool (hashes don't match)
 - your metadata is wrong
 - you need to copy the one liner as shown in this video

3) I synced the node but it's stuck on epoch 0 after restarting
 - is the CPU on 100%? It's probably reading the local db

4) How to transfer files from cloud server?
 - use SFTP (video coming)

5) Pool not showing in Daedalus
 - if metadata is correct on pooltool, give it time

6) I don't understand pool fees
 - fixed fee: taken every epoch (minimum 340 ADA)
 - margin: % of rewards that go to the pool
 - example: 1340 ADA in rewards -> 340 go to the pool, 1000 -> 2% goes to the pool, 98% -> delegator. 1000 -> delegators get 980, EDEN gets 20 ADA

7) How to secure my servers
 - video coming on SSH ports, etc
