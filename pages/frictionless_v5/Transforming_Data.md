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
steps.field_add - adiciona um campo aos dados e metadados com base nas informações fornecidas pelo usuário.

## testeeee

```python script
from frictionless import describe, Package
from pprint import pprint

# package = describe('data/*.csv')
# package.to_yaml('data/data_package.yaml')
# pprint(package.get_resource('capital-3'))
# # print(type(package))
# package = Package('data/data_package.yaml')
# print(package.get_resource('capital-3'))

# gerando um data package e imṕrimindo somente um dos recursos específicos dele
# package = Package('data/*.csv')
# package.infer(sample=False)
# pprint(package.get_resource('capital-3').read_rows())


# package.to_yaml('data/data_package2.yaml')

# p = Package('data/data_package2.yaml')
# p.infer()
# pprint(p)
# p.to_yaml('data/data_package3.yaml')

```

```python script
from frictionless import Resource

with Resource(b'header1,header2\nvalue1,value2.csv', format='csv') as resource:
  print(resource.format)
  print(resource.to_view())



```

## Teste código

```python script

from frictionless.portals import CkanControl
from frictionless.portals import CkanAdapter
from frictionless import Package
# from .control import CkanControl

ckan_control = CkanControl()
# ckan_adapter = CkanAdapter.read_package('https://dados.ufpe.br')

package = Package('https://legado.dados.gov.br/dataset/bolsa-familia-pagamentos', control=ckan_control)

```
