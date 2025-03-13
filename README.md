# Automated Unit Testing of C Code Using Python

## Overview

This script provides a robust and automated solution for unit testing C code using Python. The primary goal is to streamline the process of testing C functions by leveraging Python's `cffi` (C Foreign Function Interface) library. The script automates the extraction of C and header files from a Git repository, parses the content, generates unit testing enviroment, and produces a coverage report to ensure comprehensive testing.

## Methodology

### 1. **Configuration and Initialization**

The script begins by reading configuration settings from  `pytest.ini`. This file contains essential parameters such as the Git repository URL and the specific C and header files to be tested.

```ini
[pytest]
git_repo_url = https://github.com/example/repo.git
unittest_c_file = example.c
unittest_header_file = example.h
```

### 2. **Repository Cloning and Cleaning**

The script validates the Git repository URL and clones the repository into a designated directory. It also ensures a clean testing environment by removing any previously cloned repositories and temporary files.

### 3. **File Extraction and Parsing**

The script extracts all C and header files from all levels of the cloned repository. It then parses the content of these files to extract function declarations, structs, enums, and preprocessor directives.

```python
def extract_c_and_h_files():
    c_files = glob.glob(os.path.join(repo_path, "**/*.c"), recursive=True)
    h_files = glob.glob(os.path.join(repo_path, "**/*.h"), recursive=True)
    return c_files, h_files

def parse_content():
    h_content = read_file(hfile_path[0])
    c_content = read_file(cfile_path[0])
    structs_and_enums = extract_structs_and_enums(h_content)
    function_declarations = extract_function_declarations(h_content)
    preprocessor_directives_c = extract_preprocessor_directives(c_content)
    return parsed_h_content, parsed_c_content
```

### 4. **Test Case Generation**

Using a predefined template, the script generates unit testing enviroment. The template is populated with the parsed content, including function declarations and implementations.

```python
def create_test_file(template, header_content, c_content):
    with open(template, 'r') as f:
        template_content = f.read()
    modified_content = template_content.replace("__HEADER_DEFINTIONS__", header_content)
    modified_content = modified_content.replace("__FUNCTIONS_IMPLEMENTATION__", c_content)
    with open(f'test_{base_name}.py', 'w') as f:
        f.write(modified_content)
```

### 5. **Test Execution and Coverage Reporting**

The generated test cases are executed using Python's `unittest` framework. After execution, a coverage report is generated using `gcov` and `lcov`.

5. **Analyze Coverage Report**: The coverage report will be generated in the `coverage_report` directory. Open the `index.html` file in a web browser to view the detailed coverage report.
```python
def create_coverage_file(template):
    subprocess.run(['gcov', '_mul.c'], check=True)
    subprocess.run(['lcov', '--capture', '--directory', '.', '--output-file', 'coverage.info'], check=True)
    subprocess.run(['genhtml', 'coverage.info', '--output-directory', 'coverage_report'], check=True)
```
## Detailed Flowchart

```plaintext
Start
   |
   v
Read Configuration from pytest.ini
   |-> git_repo_url
   |-> unittest_c_file
   |-> unittest_header_file
   |
   v
Validate Git Repository URL
   |-> Check if URL starts with "https://github.com/" or "git@github.com:"
   |-> If valid -> Proceed
   |-> If invalid -> Log error and exit
   |
   v
Clean Previous Test Environment
   |-> Delete cloned repository directory
   |-> Remove temporary files (*.so, *.o, *.gcda, etc.)
   |
   v
Clone Git Repository
   |-> Clone repository into "cloned_repository" directory
   |-> Log success or failure
   |
   v
Extract C and Header Files
   |-> Recursively search for *.c and *.h files
   |-> Log number of files found
   |
   v
Parse C and Header Files
   |-> Extract function declarations
   |-> Extract structs and enums
   |-> Extract preprocessor directives
   |-> Log parsed content
   |
   v
Generate Unit Test Cases
   |-> Use unittest_testcase.template
   |-> Replace placeholders with parsed content
   |-> Save generated test file (e.g., test_example.py)
   |
   v
Compile C Code Using CFFI
   |-> Use CFFI to compile C code into a shared library
   |-> Log compilation success or failure
   |
   v
Execute Unit Tests
   |-> Run generated test cases using unittest framework
   |-> Log test results
   |
   v
Generate Coverage Report
   |-> Use gcov and lcov to generate coverage data
   |-> Create HTML coverage report in "coverage_report" directory
   |-> Log coverage report generation status
   |
   v
End
### Prerequisites

- Python 3.x
- `cffi` library (`pip install cffi`)
- `gcov` and `lcov` for coverage reporting
- Git for repository cloning

### Step-by-Step Guide

1. **Clone the Repository**: Clone the repository containing the C code you wish to test.

2. **Configure `pytest.ini`**: Update the `pytest.ini` file with the appropriate Git repository URL and the names of the C and header files to be tested.

3. **Run the Script**: Execute the script to automate the testing process.

```bash
python automated_unit_test_ETF.py
```
## References

- [Python CFFI Documentation](https://cffi.readthedocs.io/en/latest/)
- [Gcov Documentation](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html)
- [Lcov Documentation](https://linux.die.net/man/1/lcov)

---

This documentation provides a detailed overview of the automated unit testing process for C code using Python. It is designed to be accessible to beginners while providing the necessary depth for more experienced developers.
