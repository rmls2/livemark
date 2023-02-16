# Describing data 

```python script
from frictionless import describe

package = describe("data/table.csv", type="package")
print(package.to_yaml())
```

```python script
with open('data/country-1.csv') as file:
    print(file.read())
```

```python script
from frictionless import Schema

schema = Schema.describe("data/country-1.csv")
schema.to_yaml("data/country.schema.yaml") # use schema.to_json for JSON
```

```python script
with open('data/country.schema.yaml') as file:
    print(file.read())
```

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
## Describing a package

```python script
with open('data/country-3.csv') as file:
    print(file.read())
```

```python script
with open('data/capital-3.csv') as file:
    print(file.read())
```
```python script
from frictionless import describe

package = describe("data/*-3.csv")
print(package.to_yaml())
```

adcionando metadados ao pacote

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
package.to_yaml("data/country.package.yaml")

```

```python script
with open('data/country.package.yaml') as file:
    print(file.read())
```
```python script
```
## Metadata importance