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

## Fornecendo informações adcionais

```python script
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.get_field("id").title = "Identifier"
schema.get_field("neighbor_id").title = "Identifier of the neighbor"
schema.get_field("name").title = "Name of the country"
schema.get_field("population").title = "Population"
schema.get_field("population").description = "According to the year 2020's data"
schema.get_field("population").constraints["minimum"] = 0
schema.foreign_keys.append(
    {"fields": ["neighbor_id"], "reference": {"resource": "", "fields": ["id"]}}
)
schema.to_yaml("data/country.schema-full.yaml")

```

```python script
with open('data/country.schema-full.yaml') as file:
    print(file.read())

```