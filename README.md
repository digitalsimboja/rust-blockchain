# Rust Blockchain

A Proof of Work blockchain written in Rust.

Features:

- Defines data structures to model a minimum blockchain
- Mines new blocks in a separate thread, running a Proof of Work algorithm with a fixed difficulty
- Synchronizes new blocks with peer nodes in a decentralized network
- Provides a REST API to retrieve the blocks and add transactions


# Instructions to run
You will need Rust and Cargo installed.

Clone the project

```
$ git clone https://github.com/digitalsimboja/rust-blockchain.git

# Install the dependencies and run the program
$ cd rust-blockchain

# Run all the tests
$ cargo test

# Build the project in release mode
$ cargo build --release

# Run the application
$ ./target/release/rust-blockchain

```
