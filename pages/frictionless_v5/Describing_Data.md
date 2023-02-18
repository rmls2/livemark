# Describing data

```python script
from frictionless import describe

package = describe("data/table.csv", type="package")
print(package.to_yaml())
```

## Describing a Schema

```python script
with open('data/country-1.csv') as file:
    print(file.read())
```

Vamos obter um esquema de tabela usando o framework Frictionless (nota: este exemplo usa YAML para o formato do esquema, mas o Frictionless também suporta o formato JSON):

```python script
from frictionless import Schema

#vai descrever só os metadados do table schema
schema = Schema.describe("data/country-1.csv")
#vai criar o arquivo yaml para o table schema
schema.to_yaml("data/country.schema.yaml") # use schema.to_json for JSON
```

```python script
#vai abrir o yaml gerado acima
with open('data/country.schema.yaml') as file:
    print(file.read())
```

Descrevemos os nossos dados acimma, isto é geramos metadados do arquivo de dados 'country-1'. Vamos, abaixo, fornecer informações adcionais Fornecendo informações aos metadados gerados acima.

```python script
from frictionless import Schema
#descrevendo o esquema de tabela do arquivo country-1
schema = Schema.describe("data/country-1.csv")
#adcionando titulos aos campos do esquema de tabela do country-1
schema.get_field("id").title = "Identifier"
schema.get_field("neighbor_id").title = "Identifier of the neighbor"
schema.get_field("name").title = "Name of the country"
schema.get_field("population").title = "Population"
#adcionando uma descrição ao campo população do esquema de tabela do country-1
schema.get_field("population").description = "According to the year 2020's data"
#definindo uma restrição para o campo população dizendo que não pode ser menor que 0
schema.get_field("population").constraints["minimum"] = 0
#adcionando uma chave estrangeira dizendo que "neighbor_id" faz referência ao campo id e ambos estão no mesmo resource
schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
#criando o novo esquema, com as alterações feitas acima, no formato YAML
schema.to_yaml("data/country.schema-full.yaml")

```

```python script
with open('data/country.schema-full.yaml') as file:
    print(file.read())

```

## Describing a Resource

No exemplo abaixo vamos usar um arquivo csv diferente. Vamos descrever os arquivo de dados 'country-2':

```python script
with open('data/country-2.csv') as file:
    print(file.read())
```

```python script
from frictionless import describe

resource = describe('data/country-2.csv')
print(resource.to_yaml())

#descrevendo como csv
print(resource)
```

Como podemos ver houve uma inferencia errada do table schema, não existe só um campo no nosso arquivo csv country-2. O frictionless não consegue infereir metadados de uma tabela de dados muito complexa.

É possível fazer as alterações abaixo direto no yaml, mas vamos usar um script python pra isso

```python script

from frictionless import Schema, describe

resource = describe("data/country-2.csv")
#definimos as linhas de cabeçalho como a linha número 2
resource.dialect.header_rows = [2]
#Definimos o delimitador CSV como ";"
resource.dialect.get_control('csv').delimiter = ";"
#esquema que será reutilizado no country.resource.yaml
resource.schema = "country.schema.yaml"
resource.path = "country-2.csv"
#Geramos um novo resource com os metadados editados acima
resource.to_yaml("data/country.resource-cleaned.yaml")

```

note que no Data Resource abaixo o esquema será identificado por "country.schema.yaml", sem isso nosso schema ainda iria mostrar a informação errada presente no resource acima

```python script
with open('data/country.resource-cleaned.yaml') as file:
    print(file.read())

```

## Describing a package

```python script
with open('data/country-3.csv') as file:
    print(file.read())

```

```python script
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.to_yaml("country.schema.yaml")

```

```python script
from frictionless import describe

package = describe("data/*-3.csv")
print(package.to_yaml())


```

```python script
from frictionless import describe

package = describe("data/*-3.csv")
package.title = "Countries and their capitals"
package.description = "The data was collected as a research project"
package.get_resource("country-3").name = "country"
package.get_resource("capital-3").name = "capital"
package.get_resource("country").schema.foreign_keys.append(
    {"fields": ["capital_id"], "reference": {"resource": "capital", "fields": ["id"]}}
)
package.get_resource("capital").path = "capital-3.csv"
package.get_resource("country").path = "country-3.csv"

package.to_yaml("data/country.package.yaml")

```

```python script
with open('data/country.package.yaml') as file:
    print(file.read())
```

## Inferindo metadados

Muitas classes do Frictionless Framework são classes de metadados como Schema, Resource ou Package. Todas as seções abaixo são aplicáveis ​​a todas essas classes.

```python script
from frictionless import Resource

resource = Resource("data/country-1.csv")
print(resource)
```

No exemplo acima não oferecemos nenhum metadados execeto para path, por isso obtivemos o resultado esperado.

o comando resource.infer aparentemente infere os metadados adcionais do arquivo de dados dentro da classe Resource.

```python script
from pprint import pprint
from frictionless import Resource

resource = Resource("data/country-1.csv")
resource.infer(stats=False)

print(f"{resource}\n")

print(resource.to_yaml())

```

o 'stats = True' só vai servir para mostrar estatísticas o infer sozinho vai inferir metadados.  
Sem o método 'infer()' o único metadado gerado com o resource será o caminho do arquivo  
As classes de metadados Package e Schema também possui o método 'infer'.

## Validando metadados

```python script
import yaml
from frictionless import Resource

descriptor = {}
descriptor['path'] = 'data/country-1.csv'
descriptor['title'] = 1
try:
    Resource(descriptor)
# Execption é um tipo de exceção que abarca qualquer exeção sem necessidade especificar
except Exception as exception:
    print(exception.error)
    #especifica o erro, nesse caso do descriptor
    print(exception.reasons)

```

## Transformando metadados

```python script
from frictionless import Resource

resource = Resource("data/country.resource-cleaned.yaml")
resource.title = "Countries"
resource.description = "It's a research project"
# resource.dialect.header_rows = [2]
resource.to_yaml("data/country.resource-updated.yaml")

```

```python script
with open('data/country.resource-updated.yaml') as file:
    print(file.read())
```

```python script
from frictionless import Resource

resource = Resource("data/country.resource-updated.yaml")
resource.custom["customKey1"] = "Value1"
resource.custom["customKey2"] = "Value2"
resource.to_yaml("data/country.resource-updated2.yaml")

```

```python script
with open('data/country.resource-updated2.yaml') as file:
    print(file.read())

```
