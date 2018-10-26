# NKN overview

## Network
There are two main types of devices in NKN as nodes and clients:
- `Node`: a device that send, receive, and most importantly relay data. 
- `Client`: a device that only sends and receives data but does not relay data.

Through the nodes, the `clients` are able to interact with others. The `node` is the coordinator and builder of NKN, it also gains NKN token rewards through a useful proof of work. A `node` is both a data forwarder and a consensus participant, the consensus algorithm based on Cellular Automata is used to communicate with neighbors between nodes, and a consensus between large-scale nodes can be effectively achieved.

## Proof of Relay and consensus
PoR is a useful proof of work, it redefines mining as relaying data. PoR's implementation is basded on a special `signature chain`. In simple terms, the PoR signature chain is a hash chain that relay nodes sign in turn when relaying data packets. 


A signature chain is a chain of signature, signed by data relayers sequentially when relaying NKN packet. Each element of the chain consists of the following fields:

1. Relayer NKN address and public key.
2. Next relayer NKN address.
3. Signature(signature of the previous element on chain, relayer NKN address, next relayer NKN address) signed with relayer private key.

The first element of the signature chain is signed by source, and the signature field is replaced by signature(payload hash, payload size, source NKN address and public key, destination NKN address and public key, next relayer NKN address).
Signature chain cannot be forged because each element contains the NKN address and public key of the next node.  If a malicious node is on the route and intends to modify some previous elements on the chain when generating his signature, the chain is no longer valid.

New block is added to the blockchain by first selecting a `“leader”` that proposes the new block. The leader selection is uncontrollable but verifiable. The expected probability that a node is elected as leader is proportional to the data relayed by the node on secure paths.

### Leader selection procedure
`Leader` is selected by first selecting a signature chain on a secure path. The signature chain being selected is the one that has the lowest last signature value. To select the signature chain with lowest last signature, each node who signs the last signature checks if the last signature is smaller than a threshold. If the last signature is smaller than the threshold, the node sends out the signature chain to the network as a candidate for the lowest last signature.

### Block creation
Block is proposed by the selected leader in each round. Proposed block is sent to all nodes for verification and consensus. During consensus phase, each node sends to its neighbors the hash of the block (in case the leader sends out different blocks to different neighbors) and if it is accepted or not. 

In the PoR algorithm, the bookkeeper candidate vote is not a vote of a person or IP address, but a vote of a signature chain. If any malicious node wants to control NKN, it must have enough active channels (signature chain) under control. The larger scale of NKN the more difficult it is to control. 

## Reference
- [Deep dive of NKN](https://medium.com/nknetwork/deep-dive-into-nkn-system-architecture-41c0ac4c925e)
- [Proof of Relay](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Proof-of-Relay-%28PoR%29)
- [NKN consensus](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Consensus-and-Blockchain)
