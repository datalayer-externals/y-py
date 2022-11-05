# Ypy

Ypy is a Python binding for Y-CRDT. It provides distributed data types that enable real-time collaboration between devices. Ypy can sync data with any other platform that has a Y-CRDT binding, allowing for seamless cross-domain communication. The library is a thin wrapper around Yrs, taking advantage of the safety and performance of Rust.

> 🧪 Project is still experimental. Expect the API to change before a version 1.0 stable release.

## Installation

```
pip install y-py
```

## Getting Started

Ypy provides many of the same shared data types as [Yjs](https://docs.yjs.dev/). All objects are shared within a `YDoc` and get modified within a transaction block.

```python
import y_py as Y

d1 = Y.YDoc()
# Create a new YText object in the YDoc
text = d1.get_text('test')
# Start a transaction in order to update the text
with d1.begin_transaction() as txn:
    # Add text contents
    text.extend(txn, "hello world!")

# Create another document
d2 = Y.YDoc()
# Share state with the original document
state_vector = Y.encode_state_vector(d2)
diff = Y.encode_state_as_update(d1, state_vector)
Y.apply_update(d2, diff)

value = str(d2.get_text('test'))

assert value == "hello world!"
```

## Development Setup

0. [Install Rust](https://www.rust-lang.org/tools/install) and [Python](https://www.python.org/downloads/) 
  - Some alternative bootstrapping options are [pyenv](https://github.com/pyenv/pyenv) for Python, or using [mamba](https://github.com/conda-forge/miniforge) to `mamba install rust python`.
1. Install [hatch](https://hatch.pypa.io/latest/install/) (`python -m pip install hatch`)
2. Create a development build of the library

*Make sure to `unset CONDA_PREFIX` if you are in a conda environment. If `hatch` is not found on your PATH, you can run `python -m hatch` instead*
```
hatch run maturin develop
```

## Tests

All tests are located in `/tests`. There is a `test` environment matrix defined in `pyproject.toml` that will run `pytest` against `py37` through `py310`.

```
hatch run test:pytest
```

## Build Ypy :

Build the library as a wheel and store them in `target/wheels`:

```
hatch run maturin build
```
