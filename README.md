Objective: Build a Gossip protocol over a peer-to-peer network to broadcast messages and check
the liveness of connected peers. Here “liveness” means that the peer is up and running, and is able
to respond to messages sent to it.
Overview: Each peer must connect to at least b(n/2)c+1 seeds out of n available seeds. Note that
n will vary for each instance of the peer-to-peer network and the config file should be changed accord-
ingly. Config file contains the details (IP Address:Port) of seeds. The definitions of ”seed”,”peer”
etc. are given below. Each ”peer” in the network must be connected to a randomly chosen subset
of other peers. The network must be connected, that is the graph of peers should be a connected
graph. In order to bootstrap the whole process, the network should have more than one Seed node
which has information (such as IP address and Port Numbers) about other peers in the network.
Any new node first connects to seed nodes to get information about other peers, and then connects
to a subset of these.

Network setup

Network will consist of the following type of nodes:
1. Seed Nodes: A Seed node is a node that is used by a peer to get into the P2P network. There
should be n (n>1) seed nodes active, this number n will be chosen arbitrarily for different
instances of P2P network. A Seed node maintains a list of IP address and port number pairs
of peers that have connected to it. This list is called Peer List (PL). On startup, any new peer
registers itself with at least b(n/2)c + 1 randomly chosen seed nodes. Registration request
triggers an event at the seed node to add an entry containing the peer’s IP and port number
to the PL. For simplicity, in this project, seed nodes are not peers, that is, they do not perform
actions of peers as described below. Seed Node helps in building a Peer to Peer Network by
giving details about already registered Peer Nodes to new Peer Nodes.
When a seed receives a ‘dead node’ message (details given below) from any peer, it should
remove the dead node’s details from the PL if present.
(This is implemented through seed.py)
3. Peer Nodes: On launching the peer node, it first reads the IP Addresses and Port number of
seeds from a config file. The IP Addresses and Port Numbers of the seeds will be hardcoded
in this config file.(config.txt)

The peer then randomly chooses b(n/2)c+ 1 seed nodes and registers its identity (IP address:
port) with them. After registration with the chosen seed nodes, a peer requests the list of
peers available at the seed node. In response, the seed nodes send their peer list. The peer
node then takes a union of all the peer lists that it received from different seeds and randomly
selects a maximum of 4 distinct peer nodes and establishes a TCP connection with each of
them.
Along with this, every node maintains a data structure called Message List(ML) to efficiently
broadcast a gossip message so that every message travels through a link (pair of connected
peers) at most once.
Also, the peer nodes have to check for the liveness of its connected peers periodically. So a
node will send a liveness request message to all the peers it is connected to every 13 seconds.
(implemented in peer.py)
