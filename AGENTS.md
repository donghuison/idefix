# Repository Guidelines

## Project Structure & Module Organization

`src/` contains the C++ solver and Kokkos-backed core, organized into areas such as fluid, gravity, MPI, and output. `test/` holds physics and utility cases, each with its own setup and configuration files. Python test-generation and analysis tools live in `pytools/`, with unit tests in `pytools/tests/`. Build helpers are in `cmake/` and `scripts/`; documentation sources are in `doc/`. `reference/` is a submodule containing expected outputs for regression checks.

## Build, Test, and Development Commands

Build from a case directory so CMake picks up that case's `setup.cpp` and definitions:

```sh
export IDEFIX_DIR="$PWD"
cd test/HD/sod
cmake "$IDEFIX_DIR"
cmake --build . -j8
./idefix
```

CMake 3.16 or newer is required. Run Python helper tests with `python3 -m pytest pytools/tests`. To validate all configurations for one case, run `./testme.py -all` from that case directory. The smoke suite is `cd test && ./checks_examples.sh`. Reference comparisons need the submodules initialized (`git submodule update --init --recursive`) and Python dependencies from `test/python_requirements.txt`.

## Coding Style & Naming Conventions

C++ follows Google-style conventions checked by `cpplint` using `CPPLINT.cfg`; use `.hpp` headers and `.cpp` source files. Python formatting and lint rules are configured in `ruff.toml` for Python 3.10. Install hooks with `python3 -m pip install pre-commit && pre-commit install`; run `pre-commit run --all-files` before submitting. `cpplint` reports issues for manual fixes, while some hooks may edit files.

## Testing Guidelines

Name Python unit tests `test_*.py` and test functions `test_*`. Add or update a case's `testme.json` when defining build/run/reference-check combinations. Include the relevant case-level or helper test command in your change notes.

## Commit & Pull Request Guidelines

Recent history uses short, descriptive imperative subjects, sometimes with an issue or pull request number; no strict prefix is apparent. Keep each commit focused. A pull request should explain the behavior changed, list the cases or configurations exercised, and link related issues when applicable.
