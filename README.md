# YAMLConfig

Simple and basic module for reading yaml config files and creating human readable errors.

### Requirements
- pydantic
- pyyaml

### Code
```py
from pydantic import BaseModel, ValidationError
import yaml


class ConfigException(Exception):
    pass

class ConfigModel(BaseModel):
    pass

def get_config(config_path: str, config_model: ConfigModel) -> ConfigModel:
    with open(config_path, 'r') as file:
        config = yaml.safe_load(file)
    try:
        return config_model(**config)
    except ValidationError as e:
        errors = []
        for error in e.errors():
            errors.append(f'[{error["type"]}] {error["loc"][0]}: {error["msg"]}')

        raise ConfigException(f'Error loading config:\n{"\n".join(errors)}')
    
```

### Example:
```py
class Config(ConfigModel):
    url: str
    port: int
    username: str
    password: str

config = get_config('config.yaml', Config)
```
