#  RFC|BB|L0-09: Hashing algorithm improvements
* Status: `Brainstorm`

## Abstract
<!-- Full description here: https://docs.google.com/document/d/1zjJCZel8zJzgK3XuHK0YZlNffEHThq7tUOssGgRTryY/edit#heading=h.6qnrq913vou6 -->

Every time Bitswap receives a new block, [it generates the CID from the payload of the block](https://github.com/adlrocha/go-bitswap/blob/fad1a007cf9bc4f7e8e3f182a4645df60a88a9c6/message/message.go#L222) in order to verify that it belongs to a block it has in its wantlists. This means computing a lot of hash functions. This may involve a significant overhead.

## Description
Exploring more efficient implementation of hash functions, or alternative hash algorithms to fit different hardware architectures could remove an important overhead for Bitswap (and other modules from the IPFS ecosystem).

## Implementation plan
- [ ] Evaluate the overhead of hashing every block in Bitswap. This can be done by exchaching a large file and precompute the CIDs so computing the CID for every block is not needed.
- [ ] If we see that the overhead from hashing every block is significant, explore other hash functions and make a Bitswap implementation able to support other hash algorithms. Perform the same evalution from above and check the difference in the overhead.

# Impact
- Reduction in the Bitswap protocol overhead. The protocol runs faster.

## Evaluation Plan

-   [The IPFS File Transfer benchmarks.](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl)

-   Measurement of the overhead for different file exchanges for different hash algorithms.

## Prior Work
- https://github.com/minio/blake2b-simd

## Results

## Future Work
