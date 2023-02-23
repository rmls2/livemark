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

A classe Resource fornece metadados de um recurso de dados possui funções de leitura e de fluxo. As funções extract leem linhas na memória; o Resource faz o mesmo mas oferece três opçoes de saída de dados: rows, data, textou bytes.

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_bytes())

```

É uma representação textual do conteúdo:

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_text())

```

para dados tabulares representação bruta do conteúdo tabular:

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_cells())

```

Para dados tabulares, há linhas disponíveis que são listas normalizadas apresentadas como dicionários:

```python script
from frictionless import Resource

resource = Resource('data/country-3.csv')
pprint(resource.read_rows())


```

note que o método read_rows do Resource, faz o mesmo que o método extract,

Para dados tabulares é possível ler somente o cabeçalho

```python script
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    pprint(resource.header)
```

### Interfaces de transmissão

É muito útil ler todos os seus dados na memória, mas nem sempre é possível se um arquivo for muito grande. Para tais casos, o Frictionless fornece funções de streaming:

```python script
from frictionless import Resource

with Resource('data/country-3.csv') as resource:
    resource.byte_stream
    resource.text_stream
    # resource.list_stream #esse método não existe na classe Resource
    resource.row_stream
```

## Package Class

Essa classe provê funções para ler o conteúdo de um pacote. Criaremos abaixo um descrito de pacote, dessa vez usando json:

```python script
from frictionless import describe

package = describe('data/*-3.csv')
package.to_json('data/country.package.json')
```

Podemos criar um pacote a partir de arquivos de dados (usando seus caminhos) e então ler os recursos do pacote:

```python script
from frictionless import Package

package = Package('data/*-3.csv')
package.infer(sample=False)
pprint(package.get_resource('country-3').read_rows())
pprint(package.get_resource('capital-3').read_rows())
```

O pacote por si só não fornece nenhuma função de leitura diretamente porque é apenas um contrainer. Você pode selecionar o recurso de um pacakge e usar a API de recursos acima para leitura de dados.

```python script
from frictionless import Package
from pprint import pprint
"""
package = Package('data/country.package.yaml')
pprint(package.describe()) """

```

## código teste

```python script
# duas formas diferentes de gerar metadados de um data resource
""" from frictionless import Resource, extract
from pprint import pprint

resource = Resource('data/capital-3.csv')
resource.infer()
pprint(resource)

resource = describe('data/capital-3.csv')
pprint(resource) """
```

```python script








""" from pprint import pprint
from frictionless import extract

rows1 = extract('data/capital-3.yaml')
pprint(rows1)
print('')
rows2 = extract('data/capital-3.csv')
pprint(rows2) """

""" from frictionless import Resource

resource = Resource('data/capital-3.csv')
resource.infer()
resource.to_yaml('data/capital-3.yaml') """

```
