[![Test Status](https://github.com/infopulse-qa/API-Approval-Test-Project/actions/workflows/test.yml/badge.svg)](https://github.com/infopulse-qa/API-Approval-Test-Project/actions/workflows/test.yml)
# Approval test project example

Inspired by https://approvaltests.com/

## goals

Test project is using **approval testing** approach. This approach declares to collect actual results during 1st run and
use them as a golden example instead of defining expected results in advance.   
This approach can be good for API migration testing, when you need to test new API using old API responses as expected
results.
This test approach can extend regular tests, but not replace them.

## Why don't we use approval tests framework?

- Approval tests framework creates a lot of files with actual results. It is not convenient to manage and store them
- Approval tests framework saves expected results as plain text. It is not convenient to work with JSON objects
- Approval tests framework does not allow to ignore some fields in JSON objects. It is a blocker in case data contains
  autogenerated fields like timestamps, ids, etc.

## Tools

- [python 3.10+](https://www.python.org/)
- [pytest](https://docs.pytest.org/)
- [requests](https://pypi.org/project/requests/)
- [tinydb](https://tinydb.readthedocs.io/en/latest/)

⚠️ Caution! TinyDB does not support access from multiple processes or threads, so **do not** run tests in parallel

## Setup

1. clone the project
2. install dependencies using the terminal command `pip install .`
3. create the file .env in the project root folder with the following content:

```ini
SECRET_TOKEN = 'SECRET_TOKEN'
```

## About the .env file

.env file is used to store secret data like tokens, passwords, etc. It is not tracked by git, so you can safely store
your secrets there.  
Configure ENV variables in your CI/CD system to pass secret data to the tests.

## Project Description

- all tests following approval testing approach must use `verify` fixture (example below)
- to make testing rest API easier, `session` fixture introduced. No need to put complete url, endpoint would be enough
- all helpers are located in `conftest.py` file to demonstrate how tests works. They can be moved to separate files

## Approval test example

```python
def test_sample(rest, verify):
    response = rest.get('/ws/api?param=1')
    data = response.json()  # get response data in json format
    verify(data)  # if no data saved previously, current response will be saved and used to assert during next runs
```

Check [test_post.py](tests/test_posts.py) file for more examples
