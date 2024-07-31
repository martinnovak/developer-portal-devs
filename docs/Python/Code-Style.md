# Coding standards and principles

These are guidelines for Easy Chain project.

## General rules

- If we have something in MR that should be addressed in the future, add PBI immediately.

## Project structure

- Routes are treated as controllers
- Pipelines are treated as services
- API classes are used as a structure for communication with clients

## Routes

- HTTP methods should return only one type

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

Wrong example:

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

## Code


### Avoid top level code execution

Always call functions from another functions. Try not to call stuff from top level.
With exception of FastAPI initialization.

- Do not use construction of object and then call function in the same line.

Wrong code

```python
Indexer().index_files()
```

Use instead

```python
indexer = Indexer()
indexer.index_files()
```

