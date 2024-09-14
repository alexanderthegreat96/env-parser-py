## EnvParser
EnvParser is a `dependency free` utility written in Python that aims to simplify how we work with `env files`.
In other words, it will parse `env variables` and convert them to a specific type specified by the developer
or perform auto-conversion. It also escapes values, removes whitespaces and some other stuff. It's easy to use.
## Usage
 - Import the class / module
 - Initialize it with your `env_file_name`
 - Start grabbing your data

### Example
```env
# Sample ENV File 
# A string variable for database URL
DATABASE_URL="postgres://user:password@localhost/dbname"

# An integer variable for maximum connections
MAX_CONNECTIONS=10

# A boolean variable for debug mode
DEBUG_MODE=true

# A list variable for allowed hosts
ALLOWED_HOSTS="[localhost, example.com]"

# A dictionary variable for configuration settings
CONFIG={"setting1": "value1", "setting2": 2, "setting3": [1, 2, 3]}

```
```python
from EnvParser import EnvParser

env = EnvParser(".env")

# using it with individual variables
db_url = env_parser.get("DATABASE_URL", kind="string")
max_connections = env_parser.get("MAX_CONNECTIONS", kind="int", default=10)
debug_mode = env_parser.get("DEBUG_MODE", kind="bool")
allowed_hosts = env_parser.get("ALLOWED_HOSTS", kind="list")
config = env_parser.get("CONFIG", kind="dict")

print(f"Database URL: {db_url}")
print(f"Max Connections: {max_connections}")
print(f"Debug Mode: {debug_mode}")
print(f"Allowed Hosts: {allowed_hosts}")
print(f"Config: {config}")

# Retrieve all variables
all_vars = env_parser.get_vars()
print("All Variables:", all_vars)
```
## Class Documentation
### Attributes

- **`__env_file` (str)**: 
  - The path to the environment file to be parsed. Defaults to `.env`.

- **`__env` (dict or None)**: 
  - A dictionary storing environment variables after parsing. Set to `None` if parsing failed.

- **`__env_error` (str or None)**: 
  - An error message if parsing failed. Set to `None` if there were no errors.

- **`__accepted_types` (list)**: 
  - A list of accepted types for environment variable conversion. Includes `str`, `bool`, `float`, `int`, `list`, `tuple`, and `dict`.

### Methods

#### `__init__(self, env_file_path: str = ".env") -> None`

Initializes the `EnvParser` instance with the specified environment file path.

**Parameters:**
- `env_file_path` (str, optional): 
  - The path to the environment file. Defaults to `.env`.

**Attributes Initialized:**
- `__env_file`: Set to the provided file path.
- `__env`: Initially set to `None`; will be populated with parsed environment variables.
- `__env_error`: Initially set to `None`; will be populated with an error message if parsing fails.
- `__accepted_types`: List of accepted types for conversion.

#### `__parse(self) -> None`

Parses the environment file and initializes environment variables and error state.

**Behavior:**
- Reads the environment file specified by `self.__env_file`.
- Uses the `parse_env_file` function to parse the file contents.
- Sets `self.__env` with the parsed environment variables.
- Sets `self.__env_error` with an error message if parsing fails.

#### `get_error(self) -> any`

Returns the error message if the environment file parsing failed.

**Returns:**
- `any`: The error message if parsing failed; otherwise, `None`.

#### `get_vars(self) -> dict`

Retrieves all environment variables from the internal environment dictionary and converts their values.

**Returns:**
- `dict`: A dictionary where each key is an environment variable name and each value is the converted value of that environment variable.

**Notes:**
- Returns an empty dictionary if `self.__env_error` is `True` or if there are no environment variables.

#### `get(self, which: str, kind: str = "string", default: any = None) -> any`

Retrieves a value from the environment configuration and converts it to the desired type.

**Parameters:**
- `which` (str): 
  - The key to look up in the environment dictionary.
  
- `kind` (str, optional): 
  - The type to convert the value to. Defaults to `"string"`. Accepted types include:
    - `'str'` or `'string'`
    - `'bool'` or `'boolean'`
    - `'float'`
    - `'int'` or `'integer'`
    - `'list'` or `'array'`
    - `'tuple'`
    - `'dict'` or `'map'`
  
- `default` (any, optional): 
  - The value to return if the key is not found. Defaults to `None`.

**Returns:**
- `any`: 
  - The value retrieved from the environment, converted to the specified `kind` if found, or the `default` value if the key doesn't exist. Returns `None` if the type conversion fails or the key is not found.

**Notes:**
- Uses `convert_to_specific_type` for type conversion if the `kind` is specified and is in `self.__accepted_types`.
- Returns `None` if `self.__env_error` is `True`, indicating a problem with the environment configuration.


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
