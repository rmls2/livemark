# Frictionless

## Exibindo um conjunto de dados bagunçado

```python script
with open('data/invalid.csv') as file:
    print(file.read())
```

```python task id=data-transform
print('Data Transform')
```

## Describe

Descrever os dados, se tratando de frictionless, significa criar metadados para seus arquivos de dados. Por si só, os arquivos de dados não possuem informações adicionais que ajudem a compreender os dados.

Um arquivo no formato CSV sem os metadados carece de informações críticas como significado dos campos, tipos de dados, restrições, relações e etc...

Descrevemos os dados utilizando a função describe()

O trecho abaixo descreve o arquivo "invalid.csv"

```python script
from pprint import pprint
from frictionless import describe

resource = describe('data/invalid.csv')
pprint(resource)
```

## Extract

Após entendermos o formato do arquivo atraveś do esquema de tabela gerado pelo describe, podemos extraí-lo para ler o aquivo tabular normalizado do seu csv de origem.

``` python script
from pprint import pprint
from frictionless import extract

rows = extract('data/invalid.csv')
pprint(rows)
```

## Validation

A validação gerará pra nós um relatório com os erros presentes no nosso arquivo tabular. 

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/invalid.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```



## Exemplo 1

vamos exibir um dataset sobre países da europa. Um dataset coletado que possui varios erros de campo..

```python script
with open('data/countries.csv') as file:
    print(file.read())
```
### Describe exemplo 1

```python script
from pprint import pprint
from frictionless import describe

resource = describe('data/countries.csv')
pprint(resource)
```

### Update the metadata (exemplo 1)
```python script
from frictionless import Detector, describe

# detector = Detector(field_missing_values=["", "n/a"])
# resource = describe("data/countries.csv", detector=detector)
# resource.schema.get_field("neighbor_id").type = "integer"
# resource.schema.foreign_keys.append(
#     {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
# )
# resource.path = "countries.csv"
# resource.to_yaml("data/countries.resource.yaml")
```

### Abrindo o yaml com os metadados gerados (exemplo 1)
```python script
with open('data/countries.resource.yaml') as file:
    print(file.read())
```

### Extract exemplo 1

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.csv')
pprint(rows)
```

```python script
from pprint import pprint
from frictionless import extract

rows = extract('data/countries.resource.yaml')
pprint(rows)
```

### Validate exemplo 1

validando o arquivo csv.

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.csv')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/countries.resource.yaml')
pprint(report.flatten(["rowPosition", "fieldPosition", "code"]))
```