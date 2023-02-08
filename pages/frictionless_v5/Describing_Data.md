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
#descrevendo o esquema
schema = Schema.describe("data/country-1.csv")
#adcionando titulos aos campos
schema.get_field("id").title = "Identifier"
schema.get_field("neighbor_id").title = "Identifier of the neighbor"
schema.get_field("name").title = "Name of the country"
schema.get_field("population").title = "Population"
#adcionando uma descrição ao campo população
schema.get_field("population").description = "According to the year 2020's data"
#definindo uma restrição para o campo população dizendo que não pode ser menor que 0
schema.get_field("population").constraints["minimum"] = 0
#adcionando uma chave estrangeira dizendo que "neighbor_id" deve estar presente no campo "ID" 
schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
schema.to_yaml("data/country.schema-full.yaml")

```

```python script
with open('data/country.schema-full.yaml') as file:
    print(file.read())

```

## Describing a Resource

```python script
with open('data/country-2.csv') as file:
    print(file.read())
```

Vamos descrever os arquivo de dados 'country-2':

```python script
from frictionless import describe

resource = describe('data/country-2.csv')
print(resource.to_yaml())

```

## Describing a package

```python script
with open('data/country-3.csv') as file:
    print(file.read())

```