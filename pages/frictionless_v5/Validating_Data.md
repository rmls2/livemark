# Validating Data

A validação de dados tabulares é um processo de identificação de problemas que estão nos seus dados para que possamos corrigir.
Vamos olhar a seguir um exemplo de arquivo de dados:

```python script
with open('data/capital-invalid.csv') as file:
    print(file.read())
```

o frictionless fornce detalhes de erros abrangentes para que os erros possam ser compreendidos pelo usuário.
vamos usar a função de alto nível, validate, para validar o arquivo acima. Mas também é possível validar usando o CLI.

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/capital-invalid.csv')
print(report)

```

## Validating functions

A interface de alto nível para validar os dados fornecidos pelo Frictionless é um conjunto de funções validate.
são elas: validate (detecta o tipo de fonte do arquivo e valida os dados de acordo com a fonte) Schema.validate_descriptor, resource.validate, package.validate. Todos os outros validam de acordo com suas classes, execto esse aqui: inquiry.validate

```python script
import yaml
from frictionless import Schema

descriptor = {}
descriptor['fields'] = 'badd' # must be a list
with open('data/bad.schema.yaml', 'w') as file:
    yaml.dump(descriptor, file)

```

```python script
from pprint import pprint
from frictionless import validate

report = validate('data/bad.schema.yaml')
pprint(report)
```

vimos que o esquema é inválido e o erro é exibido. A validação de esquemas é útil quando vc está trabalhando  
com diferentes classes de tabela e quer criar esquemas para elas. O validate garantirá que os metadados sejam válidos

## Validating a Resource

Um recurso é um container contendo metadados e dados. Precisamos criar um descritor de recurso e então podemos validá-lo.

```python script
from frictionless import describe

""" resource = describe('data/capital-invalid.csv')
resource.path = "capital-invalid.csv"
resource.to_yaml('data/capital.resource.yaml') """

```

note que validamos o arquivo capital-invalid.csv antes, agora estamos queremos validar o descritor desse arquivo,  
nesse caso é um yaml (poderia ser um json). Queremos mostrar que garantimos o mesmo resultado usando um recurso
que usando um arquivo de dados (exemplo acima)

```python script
from frictionless import validate

report = validate('data/capital.resource.yaml')
print(report)

```

por que precisamos usar um descritor de recurso se o resultado é o mesmo?  
O motivo é metadados + empacotamento de dados. Ficará mais claro no exemplo abaixo.

```python script
from frictionless import describe

""" resource = describe('data/capital-invalid.csv')
resource.add_defined('stats')  # TODO: fix and remove this line
resource.stats.md5 = 'ae23c74693ca2d3f0e38b9ba3570775b' # this is a made up incorrect
resource.stats.bytes = 100 # this is wrong
resource.path = "capital-invalid.csv"
resource.to_yaml('data/capital.resource-bad.yaml') """

```

```python script
from frictionless import validate

report = validate('data/capital.resource-bad.yaml')
print(report)
```

Como vimos podemos adcionar novos atributos, embora errados e inválidos, mas que são detectados pelo validate. Isso ressalta a importância de um Data Resource, ou seja, de um descritor na hora de usar a validação dos dados.

## Validating a Package

Um pacote é um conjunto de recursos + metadados adicionais. Para mostrar a validação de um pacote, precisamos usar mais um arquivo tabular:

```python script
with open('data/capital-valid.csv') as file:
    print(file.read())
```

Agora vamos descrever e validar um pacote de dados que contém os arquivos de dados que vimos até agora.

```python script
from frictionless import describe, validate

# create package descriptor
package = describe("data/capital-*id.csv")
package.get_resource('capital-invalid').path = "capital-invalid.csv"
package.get_resource('capital-valid').path = "capital-valid.csv"
package.to_yaml("data/capital.package.yaml")
# validate
report = validate("data/capital.package.yaml")
print(report)
```

## Validando uma Consulta

???

## Validation Report

Todas as funções validate retornam um Relatório de Validação. Este é um objeto unificado contendo informações sobre uma validação: detalhes da fonte, o erro, etc. Vamos explorar um relatório:

```python script
from frictionless import validate

report = validate('data/capital-invalid.csv', pick_errors=['duplicate-label'])
print(report)
```

```python script

```

```python script

```

```python script

```
