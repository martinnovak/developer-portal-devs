# Coding standards and principles

These are guidelines for Easy Chain project.

## General rules

- If we have something in MR that should be addressed in the future, add PBI immediately.

## Project structure

- Routes are treated as controllers
- Pipelines are treated as services
- API classes are used as a structure for communication with clients (defined in `api.py` module)

## API

- We use REST API
    - For LLM calling, we use optional streaming which enhances UX by presenting word by word to the user.
- API structures should always inherit from Pydantic `BsaseModel` class. Use Pydantic `Field` for properly documenting
  the fields. Also, add example of API usage. They are helpful for the teams using our APIs.
- Routes are place in `routes`. They should be treated as controllers meaning:
    - All parameters/request body values should be checked here, and return corresponding HTTP error status codes (
      usually via throwing `HTTPException`). The called code should also check the parameters, but should not
      raise `HTTPException`.
- We use `/api` prefix for all our API calls.
- HTTP methods should return only one type
- Streaming should be placed in separate calls. Usually `/api/call/stream`.

## Pipelines

- They should not output API objects (maybe with exception of streaming)

## Rules for classes

- Initialize classes with parameters and fields that can be used by reusing class functionality
- Mixins are allowed but the guidelines are:
    - Use mixins only for adding generic functions that can be reused by multiple packages/modules/classes.
    - Add mixins only to extend functionality that can be used outside the class. This means no class private functions.
    - When you can name your class tool, it is not mixin.
    - In general, try to avoid mixins as inheritance is usually not the right tool.
    - Mixins are good for defining interfaces (protocols).

## Rules for methods

- Each method has well-defined purpose and is generic
- Do not use async when not calling some other async function within the function.

Don't use

```python
async def index_files(indexer: Indexer):
    indexer.index_files()
```

Use instead

```python
def index_files(indexer: Indexer):
    indexer.index_files()
```

- Methods returns only a single type (or `None`)

Don't use

```python
def fun() -> Class1 | Class2:
    ...
```

Instead, replace the code with something else. It can be either multiple functions, class inheritance (try to avoid).

## Code

### Avoid top level code execution

Always call functions from another functions. Try not to call stuff from top level.
With exception of FastAPI initialization.

- Do not use construction of object and then call function in the same line.

Don't use

```python
Indexer().index_files()
```

Use instead

```python
indexer = Indexer()
indexer.index_files()
```

