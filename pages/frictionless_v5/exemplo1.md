# Exemplos Básicos

## Exibindo o primeiro conjunto de dados: 

```python script 
with open('data/countries.csv') as file:
    print(file.read())
```
## Describe exemplo 1.

```python script 
#aplicação simples do método describe - sem consertar os metadados
from pprint import pprint
from frictionless import describe

resource = describe('data/countries.csv')
pprint(resource)
```
## Correção dos metadados usando também o método describe

```python script 
from frictionless import Detector, describe
#usa o método Detector com o argumento field_missing_values p/ determinar valores ausentes
detector = Detector(field_missing_values=["", "n/a"])
#o método detector como argumento no describe vai reconhecer os valores ausentes
resource = describe("data/countries.csv", detector=detector)
#substituição do tipo de dados do campo "neighbor_id" de string para int
resource.schema.get_field("neighbor_id").type = "integer"
#relaciona ao campo "neighbor_id" com a propria tabela como uma chave estrangeira
resource.schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
#altera o caminho do yaml gerado com os metados corrigidos
resource.path = "countries.csv"
#cria o arquivo yaml
resource.to_yaml("data/countries.resource.yaml")
``` 
## Exibindo o data package corrigido
```python script 
with open('data/countries.resource.yaml') as file:
    print(file.read())
```
## Extraindo a tabela do novo arquivo de dados com o extract
```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.resource.yaml')
pprint(rows)
```
## Validando os dados do arquivo de metadados yaml
```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.resource.yaml')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```
## Transformando dados

Vamos usar os metadados gerados para corrigir os problemas automaticamente. As únicas duas alterações manuais serão feitas no neighbor_id em 'irland' e no campo 'population' da 'france'.

```python script
from pprint import pprint
from frictionless import Resource, Pipeline, describe, transform, steps

pipeline = Pipeline(steps=[
    steps.cell_replace(field_name='neighbor_id', pattern='22', replace='2'),
    steps.cell_replace(field_name='population', pattern='n/a', replace='67'),
    steps.row_filter(formula='population'),
    steps.field_update(name='neighbor_id', descriptor={"type": "integer"}),
    steps.table_normalize(),
    steps.table_write(path="data/countries-cleaned.csv"),
])

# resource = Resource('data/countries.csv')
target = Resource('data/countries.csv').transform(pipeline)
pprint(target.read_rows())
```
## Exibindo o arquivo de dados csv corrigido

```python script
with open('data/countries-cleaned.csv') as file:
    print(file.read())
```
## Encontrando os arquivo de dados corrigido.

```python script
import os

files = [f for f in os.listdir('./data/') if os.path.isfile(path='./data/'+ f) and f.startswith('countries-cleaned')]
print(files)
```