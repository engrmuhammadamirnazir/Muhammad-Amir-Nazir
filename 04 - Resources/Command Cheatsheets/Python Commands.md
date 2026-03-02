---
type: cheatsheet
tags: [commands, python, pip, venv, cheatsheet]
created: 2026-03-02
---

# Python Commands

---

## Virtual Environments

### venv (Built-in)
```bash
python -m venv myenv
```

```bash
source myenv/bin/activate
```
> Linux/Mac activate

```bash
myenv\Scripts\activate
```
> Windows activate

```bash
deactivate
```

### conda
```bash
conda create -n myenv python=3.12
```

```bash
conda activate myenv
```

```bash
conda deactivate
```

```bash
conda env list
```

```bash
conda env remove -n myenv
```

```bash
conda env export > environment.yml
```

```bash
conda env create -f environment.yml
```

### uv (Fast Python package manager)
```bash
uv venv
```

```bash
uv pip install package
```

```bash
uv pip install -r requirements.txt
```

---

## pip

```bash
pip install package-name
```

```bash
pip install package-name==1.2.3
```

```bash
pip install package-name>=1.0,<2.0
```

```bash
pip install -r requirements.txt
```

```bash
pip install -e .
```
> Install in editable/development mode

```bash
pip install --upgrade package-name
```

```bash
pip uninstall package-name
```

```bash
pip list
```

```bash
pip list --outdated
```

```bash
pip show package-name
```

```bash
pip freeze > requirements.txt
```

```bash
pip check
```
> Verify installed packages have compatible dependencies

```bash
pip cache purge
```

```bash
pip install --no-cache-dir package-name
```

```bash
sudo pip3 install google_auth
```

```bash
sudo pip3 install qifparse
```

```bash
sudo apt-get install python3-pandas
```

---

## Running Python

```bash
python script.py
```

```bash
python -m module_name
```

```bash
python -c "print('hello')"
```

```bash
python -i script.py
```
> Run then drop into interactive shell

```bash
python -m http.server 8000
```
> Quick HTTP server

```bash
python -m json.tool file.json
```
> Pretty-print JSON

```bash
python -m venv --help
```

---

## pytest

```bash
pytest
```

```bash
pytest -v
```
> Verbose output

```bash
pytest -s
```
> Show print statements

```bash
pytest test_file.py
```

```bash
pytest test_file.py::test_function
```

```bash
pytest -k "keyword"
```
> Run tests matching keyword

```bash
pytest -x
```
> Stop on first failure

```bash
pytest --lf
```
> Run last failed tests

```bash
pytest --ff
```
> Run failed first, then rest

```bash
pytest -n auto
```
> Parallel execution (needs pytest-xdist)

```bash
pytest --cov=mypackage
```
> Code coverage

```bash
pytest --cov=mypackage --cov-report=html
```

```bash
pytest -m "slow"
```
> Run tests with marker

```bash
pytest --tb=short
```
> Short traceback

---

## Code Quality Tools

```bash
black .
```
> Format all Python files

```bash
black --check .
```
> Check formatting without changing

```bash
black --diff .
```
> Show what would change

```bash
flake8 .
```
> Lint check

```bash
flake8 --max-line-length 120 .
```

```bash
isort .
```
> Sort imports

```bash
isort --check-only .
```

```bash
mypy .
```
> Type checking

```bash
ruff check .
```
> Fast linter (replaces flake8)

```bash
ruff check --fix .
```
> Auto-fix issues

```bash
ruff format .
```
> Format (replaces black)

---

## Useful Python Modules

```bash
python -m pip install --upgrade pip
```

```bash
python -m ensurepip --upgrade
```

```bash
python -m compileall .
```
> Compile all .py to .pyc

```bash
python -m pdb script.py
```
> Debug with pdb

```bash
python -m cProfile script.py
```
> Profile script

```bash
python -m timeit "'-'.join(str(n) for n in range(100))"
```
> Benchmark expression

```bash
python -m zipfile -c archive.zip file1 file2
```

```bash
python -m site --user-site
```
> Show user site-packages path

---

*See also: [[Package Manager Commands]] | [[Linux & Bash Commands]]*
