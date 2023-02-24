# Transforming Data

## Transforming a Resource

Vamos escrever nossa primeira transformação. Aqui, transformaremos um arquivo de dados (um recurso) definindo um recurso de origem, aplicando etapas de transformação e recuperando um recurso de destino resultante: 

as funções transforme são: transform, resource.transform, packagem.transform

```python script

from frictionless import Resource, Pipeline, steps, Schema
from pprint import pprint

# Define source resource
source = Resource(path="data/transform.csv")
#schema = Schema.describe('data/transform.csv')

# Create a pipeline
pipeline = Pipeline(steps=[
    steps.table_normalize(),
    steps.field_add(name="cars", formula='population*2', descriptor={'type': 'integer'}),
])

# Apply transform pipeline
target = source.transform(pipeline)

# Print resulting schema and data
print(target.schema)
print(target.to_view())

#não faz parte do código original
print(source)

```

Vamos detalhar as etapas transformadas que aplicamos:

steps.table_normalize - lançar tipos de dados e moldar a tabela de acordo com o esquema, inferido ou fornecido 
steps.field_add - adiciona um campo aos dados e metadados com base nas informações fornecidas pelo usuário 

```python script

```

```python script
```
