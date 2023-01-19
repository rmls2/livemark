# Frictionless

## Exibindo um conjunto de dados bagunçado

```python script
with open('data/invalid.csv') as file:
    print(file.read())
```

```python task id=data-transform
print('Data Transform')
```
## Frictionless - Describe

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