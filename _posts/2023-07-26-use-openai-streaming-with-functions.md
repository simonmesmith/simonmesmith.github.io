---
layout: single
title: Use OpenAI API streaming with functions
tags: openai gpt-3.5 gpt-4 machine-learning artificial-intelligence
---

The [OpenAI API](https://platform.openai.com/docs/api-reference) provides offers several features to facilitate using powerful language models like GPT-4 and GPT 3.5.

Two very useful features are streaming and [function calling](https://openai.com/blog/function-calling-and-other-api-updates). With streaming, you give users results from the API as they're generated, which is a better user experience because users don't have to wait for an entire response at once. With function calling, you expand GPTs' capabilities with functions that you define.

But in building with the OpenAI API, I've found it challenging to combine streaming with function calling. The main reason is that GPTs stream the function calls as well as the content! Even worse, they stream function calls in pieces. So to combine streaming with function calling, you need to monitor what the models stream, output content if it's content, and build and execute function calls iteratively when it's function calls.

I created [this gist](https://gist.github.com/simonmesmith/bbeb894fc4ae954b246125eb2902800b) to do just that, and will walk through it at a high level here:

# 1. Install and configure the OpenAI library

First, perhaps obviously, you'll need to install the OpenAI library, then configure it with your API key.

```console
pip install openai
```

```python
openai.api_key = os.environ["OPENAI_API_KEY"]
```

(Here, we set the API key from environment variables for security.)

# 2. Define functions

To tell GPTs about available functions, you must define them in a way that conforms with [JSON Schema](https://json-schema.org/).

```python
FUNCTIONS = {
    "count_string": {
        "name": "count_string",
        "description": "Counts the number of characters in a string.",
        "parameters": {
            "type": "object",
            "properties": {
                "string_to_count": {
                    "type": "string",
                    "description": "The string whose characters you want to count.",
                },
            },
            "required": ["string_to_count"],
        },
    }
}
FUNCTIONS_FOR_API = list(FUNCTIONS.values())
```

In this snippet, I define a function called `count_string` that counts the number of characters in a string.

Note that I've put functions into a dictionary to make it easier to work with in `call_function` below, but also into a list, which the OpenAI API needs.

# 3. Create functions

Having defined our functions, you now must create them. In this example, I create a simple function for counting characters in a string.

```python
def count_string(string_to_count: str) -> str:
    """Counts the number of characters in a string."""
    return str(len(string_to_count))
```

# 4. Handle called functions

Next, you need some way to call functions GPTs want to execute. Here, I create a `call_function` utility. This function verifies whether the requested function is defined, validates its arguments, and calls the function, returning its result.

```python
def call_function(function_name: str, function_arguments: str) -> str:
    """Calls a function and returns the result."""
    ...
```

# 5. Manage OpenAI responses

To handle the responses from OpenAI, we define a function that checks for text or function call responses, and executes function calls as needed. Comments in the code go into greater detail about how this works.

```python
def get_response(messages: List[Dict[str, Any]]) -> Generator[str, None, None]:
    """Gets the response from OpenAI, updates the messages array, yields
    content, and calls functions as needed."""
    ...
```

# 6. Bring it all together

Finally, we use the `get_response` function to enable a conversation with streaming output and function calls.

```python
if __name__ == "__main__":
    ...
```

In this main loop, we take user input, add it to the messages array, and stream the response. If the response involves a function call, we handle it accordingly.

It's possible there's an easier way to combine streaming and function calls. It feels like there should be. But if there is, I haven't found it. Check out [the code](https://gist.github.com/simonmesmith/bbeb894fc4ae954b246125eb2902800b) and let me know if you have any suggestions.