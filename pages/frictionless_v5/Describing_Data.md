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


# package = describe("data/table.csv", type="package")
# print(package.to_yaml())
```

