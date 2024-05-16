
File I/O in Rust
================

This document explains how to create a Rust program that reads from a file and prints its contents to the console, and then modifies the program to write some text to a file.

Introduction
------------

In this task we'll cover:

1. Setting up a Rust project.
2. Writing a program that reads from a file and prints its contents to the console.
3. Modifying the program to write some text to a file.

Task 1: Reading from a File and Printing its Contents
------------------------------------------------------

### Step 1: Set Up the Rust Project

First, create a new Rust project using Cargo:

.. code-block:: bash

    $ cargo new file_RustJourney_project
    $ cd file_RustJourney_project

### Step 2: Create the File to Read

Create a file named `poem.txt` in the root directory of your project and add some text to it:

.. code-block:: text

    I'm nobody! Who are you?
    Are you nobody, too?
    Then there's a pair of us - don't tell!
    They'd banish us, you know.

    How dreary to be somebody!
    How public, like a frog
    To tell your name the livelong day
    To an admiring bog!

### Step 3: Edit `src/main.rs`

Open the `src/main.rs` file and write the following code to read from the file and print its contents:

.. code-block:: rust

    use std::env;
    use std::fs;

    fn main() {
        // Collect command-line arguments into a vector
        let args: Vec<String> = env::args().collect();

        // Save the second command-line argument (file path) in a variable
        let file_path = &args[1];

        // Print the file path for verification
        println!("In file {}", file_path);

        // Read the contents of the file specified by file_path
        let contents = fs::read_to_string(file_path)
            .expect("Should have been able to read the file");

        // Print the contents of the file
        println!("With text:\n{}", contents);
    }

### Explanation

- **Importing Modules**: We import the `env` module for handling command-line arguments and the `fs` module for file operations.
- **Collect Command-Line Arguments**: Collect all command-line arguments into a vector.
- **Save the File Path**: Save the second command-line argument (the file path) into a variable `file_path`.
- **Print File Path**: Print the file path to verify the correct path.
- **Read File Contents**: Use `fs::read_to_string(file_path)` to read the file contents into a string.
- **Print File Contents**: Print the contents of the file to the console.

### Step 4: Run the Program

Run the program with the path to `poem.txt`:

.. code-block:: bash

    $ cargo run -- poem.txt

Task 2: Writing Some Text to a File
-----------------------------------

### Step 1: Edit `src/main.rs` to Include Writing to a File

Modify `src/main.rs` to write some text to a file:

.. code-block:: rust

    use std::env;
    use std::fs::{self, OpenOptions};
    use std::io::Write;

    fn main() {
        // Collect command-line arguments into a vector
        let args: Vec<String> = env::args().collect();

        // Save the second command-line argument (file path) in a variable
        let file_path = &args[1];

        // Print the file path for verification
        println!("In file {}", file_path);

        // Read the contents of the file specified by file_path
        let contents = fs::read_to_string(file_path)
            .expect("Should have been able to read the file");

        // Print the contents of the file
        println!("With text:\n{}", contents);

        // Open a file in write mode or create it if it doesn't exist
        let mut output_file = OpenOptions::new()
            .write(true)
            .create(true)
            .append(true) // Use .truncate(true) if you want to overwrite instead of appending
            .open("output.txt")
            .expect("Could not open output.txt");

        // Text to write to the file
        let text_to_write = "Additional text to be written to the file.\n";

        // Write text to the file
        output_file.write_all(text_to_write.as_bytes())
            .expect("Failed to write to file");

        println!("Text written to output.txt");
    }

### Explanation

- **Importing Modules**: Import `OpenOptions` for advanced file opening options and `Write` for writing to files.
- **Open a File for Writing**: Use `OpenOptions` to configure file opening options and open `output.txt` for writing.
- **Text to Write**: Define the text to write to the file.
- **Write to the File**: Write the specified text to the file using `write_all`.
- **Print Confirmation**: Print a confirmation message indicating the text has been written to `output.txt`.

### Step 2: Run the Program

Run the program with the path to `poem.txt`:

.. code-block:: bash

    $ cargo run -- poem.txt

After running the program, the contents of `poem.txt` will be printed to the console, and `output.txt` will be created with the additional text written to it.

Conclusion
----------

This guide demonstrated how to read from a file and print its contents to the console, as well as how to write some text to a file in Rust. These basic file I/O operations are essential for many Rust applications and provide a foundation for more complex file handling tasks.
