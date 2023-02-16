# Extracting Data

extrair dados nada mais é do que LER dados tabulares de uma fonte.

### funções do Extract

extract - detecta o arquivo de origem e extrai os dados tabulares  
resource.extract - retorna uma tabela de dados  
package.extract - retorna um mapa das tabelas de um pacote  

extraindo um recurso de um arquivo descritor:

## Extraindo um recurso

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
from frictionless import Resource
from pprint import pprint

resource = Resource('data/capital.resource-test.yaml')
pprint(resource.extract())

``` 

No exemplo acima usamos o extract em cima do yaml para uma extração que levasse em considerações alterações no arquivo original.

## Extraindo um Pacote

A terceira maneira de extrair informações é de um pacote, que é um conjunto de dois ou mais arquivos, por exemplo, dois arquivos de dados e um arquivo de metadados correspondente. 

No exemplo abaixo forneceremos dois arquivos de dados para o extract que consiguirá indentificar que é um conjunto de dados

```python script
from pprint import pprint
from frictionless import extract

data = extract('data/*-3.csv')
pprint(data)

```

outra forma é extrair um pacote de um arquivo descritor yaml:

```python script
from frictionless import Package
from pprint import pprint

package = Package('data/country.package.yaml')
pprint(package.extract())

```

```python script

```
## Resource Class