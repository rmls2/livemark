# Frictionless básico

``` python script
with open('data/invalid.csv') as file:
    print(file.read())
```

Agora vamos usar os metados para inferir diretamente os dados tabulates do arquivo "invalid.csv". (podemos editá-lo e salvá-lo para fornecer as pessoas informações uteis sobre esses dados)


``` python script
from pprint import pprint
from frictionless import describe

resource = describe('data/invalid.csv')
pprint(resource)
```

Agora que fornecemos um esquema de tabelas do arquivo de dados, podemos usar o 'extract' para ler os dados tabulares normalizados do arquivo CSV de origem: 

``` python script
from pprint import pprint
from frictionless import extract

rows = extract('data/invalid.csv')
pprint(rows)
```

por último mas não menos importante vamos obter um relatório de validação dos dados. Isso nos ajudará a identificar e corrigir os erros presentes nos dados tabulares.

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/invalid.csv')
pprint(report.flatten(["rowNumber", "fieldNumber", "type"]))
```

