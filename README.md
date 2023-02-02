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

The application will start mining and listening on port 8000 for incoming client requests via a REST API. To change any environment variable (port, mining parameters, etc.) refer to the .env.example file.

# Client REST API

The application provides a REST API for clients to operate with the blockchain.

```
Method 	    URL 	            Description
GET 	    /blocks 	        List all blocks of the blockchain
POST 	    /blocks 	        Append a new block to the blockchain
POST 	    /transactions 	    Add a new transaction to the pool

```

The file doc/rest_api.postman.json contains a Postman collection with examples of all requests.

# Concurrency implementation

In this project, the main thread spawns three OS threads:

    - One for the miner. As mining is very computationally-intensive, we want a dedicated OS thread to not slow down other operations in the application. - In a real blockchain we would also want parallel mining (by handling a different subrange of nonces in each thread), but for simplicity we will only use one thread.
    - Other thread for the REST API. The API uses actix-web, which internally uses tokio, so it's optimized for asynchronous operations.
    - A thread for the peer system, that periodically sends and receives new blocks from peers over the network.

Thread spawning and handling is implemented using crossbeam-utils to reduce boilerplate code from the standard library.

Also, all threads share data, specifically the block list and the transaction pool. Those two data structures are implemented by using Arc<Mutex> to allow multiple concurrent writes and reads in a safe way from separate threads.
