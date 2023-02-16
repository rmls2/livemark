# Extracting Data

extrair dados nada mais é do que LER dados tabulares de uma fonte.

### funções do Extract

extract - detecta o arquivo de origem e extrai os dados tabulares
Resource.extract - retorna uma tabela de dados 
Package.extrat - retorna um mapa das tabelas de um pacote

```python script

from frictionless import Resource, extract
from pprint import pprint

resource = Resource('data/capital-3.csv')
resource.infer()
# as an example, in the next line we will append the schema
resource.schema.missing_values.append('3') # will interpret 3 as a missing value
resource.path = 'capital-3.csv'
resource.to_yaml('data/capital.resource-test.yaml') # use resource.to_json for JSON format
resource.to_json('data/capital.resource-test.json')

rows = extract('data/capital.resource-test.yaml')
pprint(rows)

```

```python script
```

```python script
```

```python script
```