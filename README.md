# nutrielody

Eliminate nutrient deficiencies using RAG-augmented LLMs to suggest additions to your meal.

## Usage

Once installed, the application can be run via the command line. Inside your virtual environment:

```bash
(.venv-nutrielody)
$ nutrielody --help
usage: nutrielody [-h] 

TBA
```

## Prerequisites

- NVIDIA RTX Card
- Linux
- Docker
- Python 3.10+
- NVIDIA Container Toolkit
- GNU make (NVIDIA offers steps in "build-from-source" if missing)

## (Optional) Set Up Rootless Docker

This increases security and limits attack surface in the case of container escape.

### Configure Docker for Rootless Mode

Follow the instructions at Docker: https://docs.docker.com/engine/security/rootless/

### Configure NVIDIA Container Toolkit for Rootless Mode

Follow the instructions from NVIDIA: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#rootless-mode

## TensorRT-LLM Setup

TensorRT-LLM provides a container for running locally-optimized inference.

Run the commands for TensorRT-LLM from the `./TensorRT-LLM` directory.

Consult the [TensorRT-LLM docs](https://nvidia.github.io/TensorRT-LLM/installation/build-from-source-linux.html#option-1-build-tensorrt-llm-in-one-step) for full walkthrough.

```bash
$ make -C docker release_build CUDA_ARCHS="89-real;90-real"
```

_:warning: This takes a long time. Go for a walk._

### TensorRT-LLM Build Issues

- `error setting rlimits for ready process: error setting rlimit type 8`
    - Sometimes your CPU will not support certain features. 
    - I had to remove the `--ulimit memlock=-1` option from the Makefile's `DOCKER_RUN_OPTS`.
- `gmake: *** [Makefile:218: tensorrt_llm] Error 2`
    - Simply trying again worked for me.
    - Otherwise, try re-cloning the [TensorRT-LLM repo](https://github.com/NVIDIA/TensorRT-LLM).

---

## Development Setup

Run all commands from the project root, unless otherwise stated.

### Virtual Environment Installation

Create the virtual environment then enter into it with `source`-ry.

#### Create the venv

You can create your venv using a provided bash script.  
This will create a new virtual environment in `$HOME/dev/.venv-nutrielody`.

```bash
$ ./create-venv.sh
```

Some may need to update the "shebang" to `#!/bin/bash` if it doesn't run.  
If you do not have bash, then what are you even doing in this section?

#### Activate the venv

Enter the virtual environment.

```bash
$ source source-me-to-activate-venv.sh
(.venv-nutrielody)
$ |
```

You should now see `(.venv-nutrielody)` before your bash prompt.

#### Confirm the venv is active

Confirm you're using the correct python with:

```bash
(.venv-nutrielody)
$ which python
=> Should show path to your ~/dev/.venv-nutrielody/Scripts/python
```

---

### Project Dependency Installation

Install the package locally from within your virtual environment.  
The `-e` flag allows changes in the filesystem to sync with the installed version. Perfect for development.

```bash
(.venv-nutrielody)
$ pip install -e .
```
Run this from the project root.

#### Install with Optional Dependencies

In the below, `".[dev]"` means you want to install the optional packages under 'dev' in [the project pyproject.toml](pyproject.toml).  
To install additional testing tools, use this pip install command instead:

```bash
(.venv-nutrielody)
$ pip install -e ".[dev]"
```

See this reference for more info about [installing Python packages](https://packaging.python.org/en/latest/tutorials/installing-packages/).

### Running Within the Virtual Environment

Now you can run the application from within the virtual environment:

```bash
(.venv-nutrielody)
$ nutrielody
```

Running the application without arguments defaults to `src/nutrielody/cli.py:main`.

---

### Test Suite

Run tests: (not yet)

```bash
(.venv-nutrielody)
$ python -m pytest -v tests/
```

---

## Logging

The application uses standard `Python logging`. All logging is to `STDERR`,
and the logging level can be set via the config file or on the command line.
Find the code in the [core module](src/nutrielody/core/).


* [TOML](https://toml.io)
* [Python logging](https://docs.python.org/3/library/logging.html)

---

