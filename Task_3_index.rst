==========================
Command Line App in Rust
==========================

Introduction
============

This document provides a step-by-step guide to creating a command line application in Rust using the `clap` crate. We will cover basic concepts, differences between `App` and `Command`, and how to use `clap_derive` for a cleaner code structure.

Concepts
========

Command Line Arguments
----------------------
When you run a program from the terminal, you can pass additional information to it. These pieces of information are known as command line arguments. In Rust, you can access these arguments using the `std::env::args` function.

Example:
    .. code-block:: rust

        use std::env;

        fn main() {
            let args: Vec<String> = env::args().collect();
            println!("{:?}", args);
        }

Conditionals in Rust
--------------------
Conditionals are used to perform different actions based on different conditions. In Rust, you typically use `if`, `else if`, and `else` statements for conditionals.

Example:
    .. code-block:: rust

        fn main() {
            let number = 3;
            if number < 5 {
                println!("The number is less than 5");
            } else if number == 5 {
                println!("The number is 5");
            } else {
                println!("The number is greater than 5");
            }
        }

Using the `clap` Library
------------------------
`clap` is a powerful library for parsing command line arguments. It provides an easy way to define, parse, and validate arguments.

Differences Between `App` and `Command`
---------------------------------------
- **`App`**: Used in `clap` version 3.x and earlier.
- **`Command`**: Used in `clap` version 4.x and later.
- The transition from `App` to `Command` is part of `clap`'s effort to provide clearer semantics and better alignment with common terminology in command-line applications.

Using `clap_derive`
-------------------
`clap_derive` provides derive macros for `clap`, making it easier to define command-line argument parsers using Rust's attribute macros.

Example:
    .. code-block:: rust

        use clap::{Parser, Subcommand};

        #[derive(Parser)]
        #[command(name = "My Program", version = "1.0", author = "Your Name <your.email@example.com>", about = "Does awesome things")]
        struct Cli {
            #[command(subcommand)]
            command: Commands,
        }

        #[derive(Subcommand)]
        enum Commands {
            Greet {
                #[arg(default_value = "World")]
                name: String,
            },
            Farewell {
                #[arg(default_value = "World")]
                name: String,
            },
        }

        fn main() {
            let cli = Cli::parse();
            match &cli.command {
                Commands::Greet { name } => println!("Hello, {}!", name),
                Commands::Farewell { name } => println!("Goodbye, {}!", name),
            }
        }

Step-by-Step Implementation
===========================

1. **Create a New Rust Project**:
    .. code-block:: shell

        cargo new my_program
        cd my_program

2. **Add Dependencies**:
    Update your `Cargo.toml`:
    .. code-block:: toml

        [dependencies]
        clap = { version = "4.1.14", features = ["derive"] }
        clap_derive = "4.1.14"

3. **Define Main Function**:
    Create `src/main.rs`:
    .. code-block:: rust

        use clap::{Arg, Command};

        fn main() {
            let matches = Command::new("My Program")
                .version("1.0")
                .author("Your Name <your.email@example.com>")
                .about("Does awesome things")
                .arg(Arg::new("command")
                    .help("The command to run")
                    .required(true)
                    .index(1))
                .arg(Arg::new("name")
                    .help("The name of the person")
                    .required(false)
                    .index(2))
                .get_matches();

            let command = matches.get_one::<String>("command").unwrap();
            let name = matches.get_one::<String>("name").unwrap_or(&"World".to_string());

            match command.as_str() {
                "greet" => println!("Hello, {}!", name),
                "farewell" => println!("Goodbye, {}!", name),
                _ => println!("Unknown command: {}", command),
            }
        }

4. **Explanation**:

    **Importing Clap**:
        .. code-block:: rust

            use clap::{Arg, Command};

        This line imports the necessary items (`Arg` and `Command`) from the `clap` crate.

    **Creating an App with Clap**:
        .. code-block:: rust

            let matches = Command::new("My Program")
                .version("1.0")
                .author("Your Name <your.email@example.com>")
                .about("Does awesome things")
                .arg(Arg::new("command")
                    .help("The command to run")
                    .required(true)
                    .index(1))
                .arg(Arg::new("name")
                    .help("The name of the person")
                    .required(false)
                    .index(2))
                .get_matches();

        - `Command::new("My Program")`: Creates a new `Command` instance with the name "My Program".
        - `.version("1.0")`: Sets the version of the program to "1.0".
        - `.author("Your Name <your.email@example.com>")`: Sets the author information.
        - `.about("Does awesome things")`: Provides a brief description of the program.
        - `.arg(Arg::new("command").help("The command to run").required(true).index(1))`: Adds a required positional argument named "command".
        - `.arg(Arg::new("name").help("The name of the person").required(false).index(2))`: Adds an optional positional argument named "name".
        - `.get_matches()`: Parses the command line arguments according to the `Command` definition and returns the matches.

    **Extracting and Using Arguments**:
        .. code-block:: rust

            let command = matches.get_one::<String>("command").unwrap();
            let name = matches.get_one::<String>("name").unwrap_or(&"World".to_string());

        - `matches.get_one::<String>("command")`: Retrieves the value of the "command" argument as a `String`.
        - `matches.get_one::<String>("name").unwrap_or(&"World".to_string())`: Retrieves the value of the "name" argument, or defaults to "World" if not provided.

    **Matching Commands**:
        .. code-block:: rust

            match command.as_str() {
                "greet" => println!("Hello, {}!", name),
                "farewell" => println!("Goodbye, {}!", name),
                _ => println!("Unknown command: {}", command),
            }

        - `match command.as_str()`: Matches the value of the "command" argument against different possible values.
        - `"greet" => println!("Hello, {}!", name)`: If the command is "greet", it prints a greeting message.
        - `"farewell" => println!("Goodbye, {}!", name)`: If the command is "farewell", it prints a farewell message.
        - `_ => println!("Unknown command: {}", command)`: If the command is anything else, it prints an unknown command message.

5. **Run the Program**:
    .. code-block:: shell

        cargo clean
        cargo build
        cargo run greet Miggy
        # Output: Hello, Miggy!

        cargo run farewell Miggy
        # Output: Goodbye, Miggy!

        cargo run greet
        # Output: Hello, Miggy!

Conclusion
==========

This guide provided a comprehensive overview of creating a command line application in Rust using `clap`. By following these steps and understanding the key concepts, you can build robust and flexible command line tools with ease.
