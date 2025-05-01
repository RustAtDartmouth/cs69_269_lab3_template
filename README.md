# :1234: CSV Summary Tool 

#### Using Rust and GitHub Actions

## Overview

In this assignment, you will develop a command-line application in **Rust** that parses a CSV (Comma-Separated Values) file and computes summary statistics such as **count**, **minimum**, **maximum**, and **average** for each numeric column. The tool should run locally your laptop, accept a filename as the only argument, and return structured output.

This assignment gives you hands-on experience with:

- Parsing structured data formats
- Implementing command-line interfaces in Rust
- Performing numerical operations and error handling
- Writing and running automated tests
- Using GitHub Actions for CI (build, test, lint, format)


---

## Learning Objectives

By completing this assignment, you will be able to:

- ✅ Read and parse CSV files using Rust and the `csv` crate
- ✅ Compute aggregate statistics from data
- ✅ Gracefully handle input errors and edge cases
- ✅ Write integration tests
- ✅ Set up continuous integration with GitHub Actions

---

## Functional Requirements

Your program must:

- Accept a single argument that is a path to a CSV file
- Read and parse the contents using the `csv` crate
- Identify all numeric columns (f64-parsable)
- Compute and print, for each numeric column:
  - Count of non-empty numeric values
  - Minimum value
  - Maximum value
  - Average value
- Print the results in a human-readable format to `stdout`

---

## Example Usage

Given the file `sample.csv`:

```csv
value1,value2,label
10,20,A
30,40,B
50,60,A
```

Then running your program with

```bash
cargo run -- sample.csv
```

the output produced would be

```bash
Summary statistics for numeric columns:
value1 -> count: 3, min: 10.00, max: 50.00, average: 30.00
value2 -> count: 3, min: 20.00, max: 60.00, average: 40.00
```

Note that `value1` and `value2` are the header columns for columns with numeric data.

## File format assumptions

- Standard `.csv` files
- The first row contains column names
- Columns may have missing values (see `sample-missing-data.csv`, line 2).

---

## Tasks

1. Copy the provided GitHub template repository

## Errors

Prevent panics !  Produce meaningful error messages to `stderr`.

## Testing

In addition to any unit-tests you have in your `.rs` source code, create an `integration_test.rs` file under the `tests/` directory of your crate. That file should 

1. Create a test `.csv` file
2. Run your binary and capture the output to verify expected values appear. Here's how to do this:

```rust
#![allow(unused_imports)]
use std::process::Command;  // needed when running test_summary_output

// main code

#[test]
fn test_summary_output() {
    let output = Command::new("cargo")
        .args(&["run", "--", "sample.csv"])
        .output()
        .expect("Failed to run");

    let stdout = String::from_utf8_lossy(&output.stdout);
    assert!(stdout.contains("value1"));
    assert!(stdout.contains("average"));
}
```

You can run this test manually with `cargo test`

## GitHub Actions CI

Setup a GitHub Actions workflow file at the top of your directory: `.github/workflows/lab3-ci.yml` . It should perform several jobs when the repository is `pushed` to either the `main` or `develop` branches. 


```githubworkflow
jobs:
  check:
    name: lint
    # Run clippy: `cargo clippy -- -D warnings`
    # ...
  formatting:
    name: fmt
    # Check source code formatting: cargo rustfmt -- --check
    # ...
  build:
	  # setup for building and testing
	  # ...
    # cargo build
    # cargo test --all-features
```

## Grading rubric (25 points)

| Feature                                   | Points |
| ----------------------------------------- | ------:|
| Reads and parses CSV file correctly      |    5    |
| Correct computation of summary statistics |    5    |
| Integration tests with `cargo test`       |    5    |
| CI workflow: build, test, lint, format    |    5    |
| Graceful error handling (especially no panics) |    5    |



## License

This project is provided for educational purposes and licensed under the MIT License.
