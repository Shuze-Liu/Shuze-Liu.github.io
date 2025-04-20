---
layout: post
title:  "optimizations for gpu"
categories: jekyll update
published: false

---
outline:
- gpu: compute-specialized hardware
- compute architecture and memory hierarchy
  - SM (register and cache), warp
- programming compute on gpu 
  - thread, block and grid
  - scheduling and synchronization
- programming memory on gpu
  - moving between memory architecture
    - collabortive loading
  - texture memory and constant memory

list of common gpu optimizations
- what triton does for you: synchronization, thread-level register fetching
- what you need to consider in triton
- other stuff beyond triton

### memory coalesce
why memory coalesce: when all threads in a warp load consecutive global memory locations, these requests will be merged to single one request. this benefits in two ways
- reduce load instruction
- avoid bank conflict
How to coalesce:
- when threads align well with memory, all good
- otherwise, match them, either by 1) reagange memory, or 2) reagainge mapping from thread to data
reagange memory: corner turning
- load stuff coalesced into shared memory, and access in any order inside that


### background
- bank conflict

# 不管学啥，先写下来，感到相关，然后最后再整理