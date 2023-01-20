# pré-processamento de dados 

[![Build](https://img.shields.io/github/actions/workflow/status/frictionlessdata/learning-python/general.yaml?branch=main)](https://github.com/frictionlessdata/learning-python/actions)
[![Codebase](https://img.shields.io/badge/codebase-github-brightgreen)](https://github.com/frictionlessdata/learning-python)
[![Support](https://img.shields.io/badge/support-discord-brightgreen)](https://discord.com/channels/695635777199145130/695635777199145133)

Aqui será o corpo do projeto.

vamos printar "Hello World" in Python:

```python script
print("Hello World")
```

```bash task id=data-extract
echo 'Data Extract'
```
```python task id=data-transform
print('Data Transform')
```
```python task id=data-load
print('Data Load')
```

```bash task id=data
Data Extract
Data Transform
Data Load
```

## Validation

```python script
from pprint import pprint
from frictionless import validate

report = validate('invalid.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

```yaml table
data: data/cars.csv
width: 600
order:
  - [3, 'desc']
columns:
  - data: type
  - data: brand
  - data: model
  - data: price
  - data: kmpl
  - data: bhp
```
```

```

```yaml chart
data:
  url: data/cars.csv
mark: circle
selection:
  brush:
    type: interval
encoding:
  x:
    type: quantitative
    field: kmpl
    scale:
     domain: [12,25]
  y:
    type: quantitative
    field: price
    scale:
     domain: [100,900]
  color:
    condition:
      selection: brush
      field: type
      type: nominal
    value: grey
  size:
    type: quantitative
    field: bhp
width: 500
height: 300
```