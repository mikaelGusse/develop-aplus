# A utility repository for beginning development with A+ and related services

## Getting started

1. Install required software
    ```
    # Aalto Linux
    pkcon install gettext python3-venv

    # Debian-based Linux with administrator permissions
    sudo apt update
    sudo apt install gettext python3-venv

    # Apple macOS
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    brew install gettext
    ```
2. Clone the repository
    ```
    git clone git@github.com:apluslms/develop-aplus.git
    ```
3. Move to the directory
    ```
    cd develop-aplus
    ```
4. Initialize the development environment and activate the Python virtual environment
    ```
    source ./init-env.sh
    ```
5. Run the Docker containers
    ```
    ./docker-up.sh
    ```

Done!

The A+ development code in the `a-plus` submodule has been mounted and your code changes in the `a-plus`
directory are now visible at http://localhost:8000/. To mount development code for the other services, edit the
`docker-compose.yml` by uncommenting the relevant lines under the `volumes` section.

After the development environment has been initialized once by running `./init-env.sh`,
the next time you only need to activate the virtual environment before running the Docker containers:

```
source .venv/bin/activate
./docker-up.sh
```

## Pushing code changes to your forked repository

The upstream/origin for the submodules [a-plus](https://github.com/apluslms/a-plus),
[mooc-grader](https://github.com/apluslms/mooc-grader), [gitmanager](https://github.com/apluslms/gitmanager),
[mooc-jutut](https://github.com/apluslms/mooc-jutut), and [aplus-manual](https://github.com/apluslms/aplus-manual)
is the [apluslms](https://github.com/apluslms) organization. You should fork the GitHub repositories that you are
working on and push code changes to your own repository first and then create a pull request.

Example:

How to add your forked `a-plus` repository to the submodule as a remote and push changes to it:

1. Move to the `a-plus` submodule directory under `develop-aplus`
    ```
    cd a-plus
    ```
2. Add your fork as a remote and give it a name, such as your GitHub username
    ```
    git remote add <myusername> git@github.com:<myusername>/a-plus.git
    git fetch <myusername>
    ```
3. Create a new branch for the new code changes
    ```
    git switch -c <mybranch>
    ```
4. Make your changes
5. Push the changes to your forked repository
    ```
    git add somefile.py
    git commit
    git push <myusername>
    ```
6. Create a GitHub pull request

## Debugging with VSCode

### How to debug

1. Open the `develop-aplus` directory in VSCode as a [VSCode Workspace](https://code.visualstudio.com/docs/editor/workspaces)
2. Run the containers with `./docker-up.sh`
3. Open the VSCode `Run and Debug tab`
4. Attach to, for example, the A+ container with the `Remote Attach to A+` debug configuration
5. Set your breakpoints in the code

### SystemExit exception

Django automatic reloads cause VSCode to stop on the SystemExit exception. This behaviour can be stopped by unchecking
the `Uncaught Exceptions` option in the breakpoints panel of the `Run and Debug tab`.

## Running tests

In the `develop-aplus` root directory, first activate the virtual environment:
```
source .venv/bin/activate
```

### A+ tests

#### Running A+ tests in VSCode

The A+ tests can be run conveniently in VSCode's `Testing tab`.

If the `Testing tab` is missing or does not immediately show any tests, make sure to first open the
`develop-aplus` directory in VSCode as a [VSCode Workspace](https://code.visualstudio.com/docs/editor/workspaces).
Next, install the
[VSCode Python extension by microsoft](https://marketplace.visualstudio.com/items?itemName=ms-python.python) and then
activate the correct Python interpreter in VSCode.
Selecting the interpreter is done by pressing `Ctrl + Shift + P` (or `Cmd + Shift + P` on macOS) and searching for
`Python: Select Interpreter`. Select the Python virtual environment interpreter located at `.venv/bin/python`.

#### Running A+ tests in terminal

In order to run tests in the terminal, first move to the `a-plus` directory:
```
cd a-plus
```

Run all tests:
```
pytest
```

Run Playwright end-to-end tests only:
```
pytest e2e_tests
```

Run and debug a specific Playwright end-to-end test:
```
PWDEBUG=1 pytest -s -k <test_method_name>
```

Run unit tests only:
```
pytest --ignore=e2e_tests
```

Run and debug a specific unit test:
```
pytest -s -k <test_method_name>
```

An alternative way to run A+ unit tests only:
```
python3 manage.py test
```


### MOOC-Grader tests

Move to the `mooc-grader` directory:
```
cd mooc-grader
```

Run all tests inside a Docker container:
```
docker-compose -f docker-compose.test.yml up grader_unit
```

### Git manager tests

Move to the `gitmanager` directory:
```
cd gitmanager
```

Run all tests inside a Docker container:
```
docker-compose -f docker-compose.test.yml up gitmanager_unit
```

## How to generate/post submissions to A+ automatically

Run `./generate-submissions.sh` in the root of this repository.

Check the [README.md](utils/submission-generator/README.md) in the `utils/submission-generator` directory.

## Updating the submodules in this repository

To keep this repository up-to-date, pull the newest changes to the submodules:
```
./update-submodules.sh
```
