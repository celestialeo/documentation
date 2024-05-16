===========================
HTTP GET Request in Rust
===========================

This documentation provides a detailed guide on how to use the `reqwest` crate to make a simple asynchronous HTTP GET request to a public API in Rust. It covers the setup, code explanation, and running the project, including using separate files for better modularization.

Contents:
---------

1. Setting Up Your Rust Project
2. Adding Dependencies
3. Writing the Code
4. Detailed Explanation of the Code
   1. main.rs
   2. http_reqwest.rs
5. Running Your Project

Setting Up Your Rust Project
----------------------------

First, if you don't already have a Rust project, create a new one using Cargo:

.. code-block:: bash

    cargo new task2_http_client
    cd task2_http_client

Adding Dependencies
-------------------

Open the `Cargo.toml` file and add the necessary dependencies:

.. code-block:: toml

    [package]
    name = "task2_http_client"
    version = "0.1.0"
    edition = "2018"

    [dependencies]
    reqwest = { version = "0.11", features = ["json"] }
    tokio = { version = "1", features = ["full"] }

Writing the Code
----------------

1. Create `src/main.rs` with the following code:

.. code-block:: rust

    mod http_reqwest;

    #[tokio::main]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
        if let Err(e) = http_reqwest::run().await {
            eprintln!("Error: {}", e);
        }
        Ok(())
    }

2. Create `src/http_reqwest.rs` with the following code:

.. code-block:: rust

    use reqwest;
    use std::error::Error;

    // Define the run function to be called from main.rs
    pub async fn run() -> Result<(), Box<dyn Error>> {
        // Define the URL you want to request
        let url = "https://api.github.com/repos/rust-lang/rust";

        // Make the GET request
        let response_text = reqwest::get(url).await?.text().await?;

        // Print the response text
        println!("Response: {}", response_text);

        Ok(())
    }

Detailed Explanation of the Code
--------------------------------

1. **main.rs**:
   - `mod http_reqwest;`: Declares a module named `http_reqwest` and includes the code from `http_reqwest.rs`.
   - `#[tokio::main]`: This attribute sets up the `tokio` runtime, allowing the `main` function to be asynchronous.
   - `async fn main() -> Result<(), Box<dyn std::error::Error>>`: Defines the `main` function as asynchronous and capable of returning a `Result` type for error handling.
   - `if let Err(e) = http_reqwest::run().await { ... }`: Calls the `run` function from the `http_reqwest` module and awaits its completion. If an error occurs, it prints the error message.
   - `Ok(())`: Indicates successful completion of the function.

2. **http_reqwest.rs**:
   - `use reqwest;`: Imports the `reqwest` crate for making HTTP requests.
   - `use std::error::Error;`: Imports the `Error` trait for error handling.
   - `pub async fn run() -> Result<(), Box<dyn Error>> { ... }`: Defines an asynchronous `run` function that returns a `Result`.
   - `let url = "https://api.github.com/repos/rust-lang/rust";`: Defines the URL for the GET request.
   - `let response_text = reqwest::get(url).await?.text().await?;`: This line makes the actual HTTP GET request and retrieves the response text.
     - `reqwest::get(url)`: Initiates an asynchronous GET request to the specified URL.
     - `.await?`: Awaits the result of the GET request. If an error occurs, it is propagated using the `?` operator.
     - `.text().await?`: Converts the response body to a string and awaits the result. If an error occurs, it is propagated using the `?` operator.
   - `println!("Response: {}", response_text);`: Prints the response text to the console.
   - `Ok(())`: Indicates successful completion of the function.

Running Your Project
--------------------

After setting up the files, build and run your project using Cargo:

.. code-block:: bash

    cargo build
    cargo run

This will make an asynchronous HTTP GET request to the GitHub API and print the response to the console, handling errors appropriately.

Summary
-------

This documentation provides a comprehensive guide to setting up and running a Rust project that makes an asynchronous HTTP GET request using the `reqwest` and `tokio` crates. By following these steps and using separate files for modularization, you can efficiently manage HTTP requests in an asynchronous manner, ensuring your application remains responsive and performant.
