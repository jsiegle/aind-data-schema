# aind-data-schema

[![License](https://img.shields.io/badge/license-MIT-brightgreen)](LICENSE)
![Code Style](https://img.shields.io/badge/code%20style-black-black)

A library that defines [AIND](https://alleninstitute.org/what-we-do/brain-science/research/allen-institute-neural-dynamics/) data schema and validates JSON files. 

## Overview

This repository contains the schemas needed to ingest and validate metadata that are essential to ensuring [AIND](https://alleninstitute.org/what-we-do/brain-science/research/allen-institute-neural-dynamics/) data collection is completely reproducible. Our general approach is to semantically version core schema classes and include those version numbers in serialized metadata so that we can flexibly evolve the schemas over time without requiring difficult data migrations. In the future, we will provide a browsable list of these classes rendered to [JSONschema](https://json-schema.org/), including all historic versions.

Be aware that this package is still under heavy preliminary development. Expect breaking change regularly, although we will communicate these through semantic versioning.

A simple example:

```python
from aind_data_schema import Subject, LightCycle

s = Subject(
    subject_id='123456',
    sex='Female',
    species='Mus musculus',
    genotype='wt/wt',
    date_of_birth='2022-01-01',
    light_cycle=LightCycle(
        lights_on_time='08:00:00',
        lights_off_time='20:00:00'
    )
)

print(s.json(indent=3))
```
{
   "describedBy": "https://github.com/AllenNeuralDynamics/data_schema/blob/main/schemas/subject.py",
   "schema_version": "0.2.0",
   "species": "Mus musculus",
   "subject_id": "123456",
   "sex": "Male",
   "date_of_birth": "2022-01-01",
   "genotype": "wt/wt",
   "background_strain": null,
   "source": null,
   "restrictions": null,
   "breeding_group": null,
   "maternal_id": null,
   "maternal_genotype": null,
   "paternal_id": null,
   "paternal_genotype": null,
   "light_cycle": {
      "lights_on_time": "08:00:00",
      "lights_off_time": "20:00:00"
   },
   "home_cage_enrichment": null,
   "notes": null
}
```

## Installing and Upgrading

To install the latest version:
```
pip install aind-data-schema
```

Every merge to the `main` branch is automatically tagged with a new major/minor/patch version and uploaded to PyPI. To upgrade to the latest version:
```
pip install aind-data-schema --upgrade
```

To develop the code, check out this repo and run the following in the cloned directory: 
```
pip install -e .[dev]
```

## Contributing

If you've found a bug in the schemas or would like to make a minor change, open an [Issue](https://github.com/AllenNeuralDynamics/aind-data-schema/issues) on this repository. If you'd like to propose a large change or addition, or generally have a question about how things work, head start a new [Discussion](https://github.com/AllenNeuralDynamics/aind-data-schema/discussions)!


### Linters and testing

There are several libraries used to run linters, check documentation, and run tests.

- Please test your changes using the **coverage** library, which will run the tests and log a coverage report:

```
coverage run -m unittest discover && coverage report
```

- Use **interrogate** to check that modules, methods, etc. have been documented thoroughly:

```
interrogate .
```

- Use **flake8** to check that code is up to standards (no unused imports, etc.):

```
flake8 .
```

- Use **black** to automatically format the code into PEP standards:

```
black .
```

- Use **isort** to automatically sort import statements:

```
isort .
```

### Pull requests

For internal members, please create a branch. For external members, please fork the repo and open a pull request from the fork. We'll primarily use [Angular](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit) style for commit messages. Roughly, they should follow the pattern:
```
<type>(<scope>): <short summary>
```

where scope (optional) describes the packages affected by the code changes and type (mandatory) is one of:

- **build**: Changes that affect the build system or external dependencies (example scopes: pyproject.toml, setup.py)
- **ci**: Changes to our CI configuration files and scripts (examples: .github/workflows/ci.yml)
- **docs**: Documentation only changes
- **feat**: A new feature
- **fix**: A bug fix
- **perf**: A code change that improves performance
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **test**: Adding missing tests or correcting existing tests

### Documentation

To generate the rst files source files for documentation, run:

```
sphinx-apidoc -o doc_template/source/ src 
```

Then to create the documentation html files, run:
```
sphinx-build -b html doc_template/source/ doc_template/build/html
```

More info on sphinx installation can be found here: https://www.sphinx-doc.org/en/master/usage/installation.html
