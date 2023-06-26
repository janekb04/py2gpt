# py2gpt

Recently, OpenAI has released the GPT function API that allows models like GPT-4 to "invoke" functions on the user's behalf.
The problem is that the format, in which these functions is specified, is JSON Schema, which is relatively niche and hard to work with.
This repository addresses this issue. It converts regular Python function declarations into JSON consumable by the OpenAI API.

For example, this Python code:
```py
def get_current_weather(location: str, format: Literal["fahrenheit", "celsius"]):
    """
    Get the current weather
    :param location: The city and state, e.g. San Francisco, CA
    :param format: The temperature unit to use. Infer this from the users location.
    """
```
Produces the following JSON that matches one of OpenAI's own examples:
```js
{
    'name': 'get_current_weather',
    'description': 'Get the current weather',
    'parameters': {
        'type': 'object',
        'properties': {
            'location': {
                'description': 'The city and state, e.g. San Francisco, CA',
                'type': 'string'
            },
            'format': {
                'description': 'The temperature unit to use. Infer this from the
users location.',
                'type': 'string',
                'enum': ('fahrenheit', 'celsius')
            }
        }
    },
    'required': ['location', 'format']
}
```

## Roadmap

- [ ] Support classes (treat them as `object`s with certain `"properties"
- [ ] Support methods (treat them as functions with an additional `self` parameter`)
- [x] Support multiple functions
- [ ] Support `Enum`s
- [ ] Support named tuples
- [ ] Make the code safe for local execution
- [ ] Refactor code and make locally executable
