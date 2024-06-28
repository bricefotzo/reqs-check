
# reqs-check

`reqs-check` is a tool to check and compare dependencies files(requirements.txt, pyproject.toml, etc...) for Python projects.


## Installation

To install `reqs-check`, use `pip`:

```sh
pip install reqs-check
```

## Usage
```
usage: reqs-check [-h] [--only-diff] [--find-duplicates] F [F ...]

Check and compare multiple dependencies files.

positional arguments:
  F                  Paths to the dependencies files

options:
  -h, --help         show this help message and exit
  --only-diff        Only show packages with differing versions
  --find-duplicates  Find duplicate packages in the dependencies files
```

## Examples

Let's consider the following files: 

**requirements1.txt**
```
pandas==1.1.5
numpy==1.19.5
scipy~=1.5.4
matplotlib==3.3.4
```
and 

**requirements2.txt**
```
pandas==1.1.5
numpy==1.19.3
matplotlib==3.2.1
requests>=2.24.0
scipy==1.5.4
numpy==1.19
requests
```
### Compare requirements files

> **Note:** The files are in the directory `examples/` of the github repository.

This command compares two `requirements.txt` files and highlights the differences.

```sh
reqs-check requirements1.txt requirements2.txt
```
**Output:**
```
+----+------------+---------------------+---------------------+
|    | Package    | requirements1.txt   | requirements2.txt   |
+====+============+=====================+=====================+
|  0 | pandas     | ==1.1.5             | ==1.1.5             |
+----+------------+---------------------+---------------------+
|  1 | numpy      | ==1.19.5            | ==1.19              |
+----+------------+---------------------+---------------------+
|  2 | scipy      | ~=1.5.4             | ==1.5.4             |
+----+------------+---------------------+---------------------+
|  3 | matplotlib | ==3.3.4             | ==3.2.1             |
+----+------------+---------------------+---------------------+
|  4 | requests   | Not Present         | Any                 |
+----+------------+---------------------+---------------------+
```
> **Note:** The values are color-coded for better readability in the terminal.

### Filter to show only packages with differing versions

This command compares two `requirements.txt` files and displays only the packages with differing versions.

```sh
reqs-check --only-diff requirements1.txt requirements2.txt 
```
**Output:**
```
+----+------------+---------------------+---------------------+
|    | Package    | requirements1.txt   | requirements2.txt   |
+====+============+=====================+=====================+
|  1 | numpy      | ==1.19.5            | ==1.19              |
+----+------------+---------------------+---------------------+
|  2 | scipy      | ~=1.5.4             | ==1.5.4             |
+----+------------+---------------------+---------------------+
|  3 | matplotlib | ==3.3.4             | ==3.2.1             |
+----+------------+---------------------+---------------------+
|  4 | requests   | Not Present         | Any                 |
+----+------------+---------------------+---------------------+
```
You can see that the output only shows the packages with differing versions, so the first package `pandas` is not displayed.


### Find duplicates in requirements files

This command finds duplicated packages in the `requirements.txt` files(at least one).

```sh
reqs-check --find-duplicates requirements1.txt requirements2.txt
```
**Output:**
```
Duplicates in requirements2.txt:
  numpy:
    - ==1.19.3 (line 2)
    - ==1.19 (line 6)
  requests:
    - >=2.24.0 (line 4)
    - Any (line 7)
````
You can see that the output shows the duplicated packages in the second file `requirements2.txt` while the first file `requirements1.txt` has no duplicated packages.

## Next Steps

We plan to add more features to `reqs-check` to resolve some use cases that we've had to deal with. 

### Planned features

- Support linting of requirements files for best practices.

- Support for additional file formats (e.g., `Pipfile`, `pyproject.toml`).

- Support for additional checks (e.g., security vulnerabilities).

- Integration with CI/CD pipelines for automated checks.

- Detailed reports in various formats (e.g., JSON, HTML).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
