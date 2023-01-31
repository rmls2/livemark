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