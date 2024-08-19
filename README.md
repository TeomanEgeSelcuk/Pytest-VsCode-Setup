# VS Code Configuration for Python Unit Testing with Pytest

This repository includes configuration files designed to streamline the process of running and debugging Python unit tests using pytest in Visual Studio Code. Below is an explanation of the contents of the `launch.json` and `settings.json` files, which are used to configure the testing environment.

## `launch.json`

The `launch.json` file contains several configurations to help you run and debug your Python code and pytest tests efficiently.

```json
{
    "version": "0.2.0",  // Specifies the version of the launch configuration format. This is standard.
    "configurations": [  // An array of configuration objects, each defining a way to run or debug your code.

        {
            "name": "Python Debugger: Current File",  // The display name for this configuration.
            "type": "debugpy",  // Specifies the debugger type. 'debugpy' is used for Python.
            "request": "launch",  // Indicates that this configuration will launch the program.
            "program": "${file}",  // The file currently open in the editor will be the program that runs.
            "console": "integratedTerminal",  // The integrated terminal in VS Code will be used to display the output.
            "env": {  // Defines environment variables for the session.
                "PYTHONPATH": "${workspaceFolder}"  // Adds the workspace folder to the Python path, allowing imports relative to the workspace root.
            }
        },
        {
            "name": "Pytest",  // Configuration for running pytest on a specific test file.
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",  // Indicates that pytest will be run as a module.
            "args": [
                "${workspaceFolder}/tests/test_${fileBasename}"  // Runs pytest on the test file corresponding to the current file in the editor.
            ],
            "justMyCode": true  // Limits debugging to user-written code, excluding external libraries.
        },
        {
            "name": "Debug Specific Pytest Case",  // Debugs a specific pytest test case.
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",
            "args": [
                "-v",  // Enables verbose output for more detailed test results.
                "tests/test_main.py::test_extract_classes_and_functions[4]"  // Specifies the exact test case to run. Replace with the actual test you want to debug.
            ],
            "console": "internalConsole",  // Uses the Debug Console in VS Code to show output.
            "justMyCode": true
        },
        {
            "name": "Debug Multiple Pytest Cases",  // Configuration for running multiple pytest cases in one go.
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",
            "args": [
                "-v",  // Enables verbose mode.
                "tests/test_main.py::test_assert_outputs[9]",  // List of specific test cases to run. Each entry points to a different test case.
                "tests/test_main.py::test_assert_outputs[10]",
                "tests/test_main.py::test_assert_outputs[11]",
                "tests/test_main.py::test_assert_outputs[12]"
            ],
            "console": "internalConsole",  // Uses the Debug Console for output.
            "justMyCode": true
        },        
        {
            "name": "Debug Entire Pytest Function",  // Debugs an entire pytest function.
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",
            "args": [
                "-v",  // Verbose mode for detailed output.
                "tests/test_main.py::test_execute"  // Specifies the test function to run. This runs all test cases within the function.
            ],
            "console": "internalConsole",
            "justMyCode": true
        },
        {
            "name": "Debug All Tests in Specific Module",  // Runs all tests in a specific module.
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",
            "args": [
                "-v",
                "tests/test_main.py"  // Replace with the path to the specific module you want to test.
            ],
            "console": "internalConsole",
            "justMyCode": true
        },
        {
            "name": "Debug All Tests in All Modules",  // Runs all tests across all modules in the 'tests' directory.
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",
            "args": [
                "-v",
                "tests/"  // This points to the directory containing all your test modules. It will run all the tests found in this directory.
            ],
            "console": "internalConsole",
            "justMyCode": true
        }
    ]
}
```

### Explanation of `launch.json`

- **`version`:** This specifies the version of the launch configuration format. The version `0.2.0` is a common format for defining debug configurations.
- **`configurations`:** This is an array of objects where each object represents a different way to launch or debug your Python code. Each configuration can be selected depending on what you need to debug (e.g., a single test file, a specific test case, or all tests).

Each configuration has several important fields:
- **`name`:** The name of the configuration as it will appear in the VS Code UI.
- **`type`:** Specifies the debugger type, with `debugpy` being the debugger for Python.
- **`request`:** This indicates whether the configuration will `launch` a new program or `attach` to an existing one.
- **`program`:** The path to the Python file you want to run. `${file}` is a variable that points to the current file in the editor.
- **`module`:** Instead of specifying a file, you can specify a module like `pytest` to run tests.
- **`args`:** A list of arguments passed to the program or module. These often include test file paths and options like `-v` for verbose output.
- **`console`:** Determines where the output will be shown. `integratedTerminal` shows it in the terminal, while `internalConsole` shows it in the Debug Console.
- **`justMyCode`:** When set to `true`, this option restricts the debugger to only step through code you wrote, ignoring library code.

## `settings.json`

The `settings.json` file contains configurations to enable and optimize pytest within your VS Code environment.

```json
{
    "python.testing.pytestArgs":[
        "--verbose",  // This argument makes the test output more detailed.
        "tests/",  // Points to the directory where your tests are located.
        "--rootdir=${workspaceFolder}"  // Sets the root directory for pytest, typically your project folder.
    ],
    "python.testing.unittestEnabled": false,  // Disables unittest as the testing framework. Only pytest will be used.
    "python.testing.pytestEnabled": true,  // Enables pytest as the testing framework.
    "python.analysis.enablePytestSupport": true,  // Enhances Python analysis with support for pytest-specific features.
    "python.testing.autoTestDiscoverOnSaveEnabled": true  // Automatically discovers and updates tests whenever a file is saved.
}
```

### Explanation of `settings.json`

- **`python.testing.pytestArgs`:** This list contains arguments passed to pytest. 
  - `--verbose`: Enables detailed output, making it easier to see what's happening during tests.
  - `tests/`: Specifies the directory where the tests are located.
  - `--rootdir=${workspaceFolder}`: Sets the root directory for pytest, allowing relative imports and test discovery based on the project root.

- **`python.testing.unittestEnabled`:** Setting this to `false` disables unittest, ensuring pytest is the only active test framework.
  
- **`python.testing.pytestEnabled`:** This enables pytest, making it the primary test framework in your project.

- **`python.analysis.enablePytestSupport`:** Enhances the Python language server's ability to understand pytest, improving features like autocomplete and error checking for test-related code.

- **`python.testing.autoTestDiscoverOnSaveEnabled`:** Automatically discovers tests when you save a file, ensuring your test explorer is always up to date.

## Usage

With these configurations in place, you can easily run and debug your Python unit tests using pytest in Visual Studio Code. The `launch.json` configurations allow for flexible debugging options, while the `settings.json` ensures that pytest is fully integrated into your workflow.

### Running Tests

To run tests, open the test file or module in Visual Studio Code, and use the `Run Test` or `Debug Test` options that appear above the test functions or in the testing sidebar.

### Debugging Tests

Use the various debugging configurations in `launch.json` to debug specific tests, multiple tests, or all tests within a module or directory.

---
