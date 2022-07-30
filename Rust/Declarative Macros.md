## Declarative Macros
Macros allow you to write code which writes other code (meta programming).

Can be though of like a function where **input is code** and **output is code** that is transformed in some way.

 
|Function|Macros|
|---|----|
|Fixed num of params| Variable num of params|
|Called at runtime| Expanded during compilation|
>However, Macros introduce new complexity as they write code that write's other code (harder to debug)

2 Types of Macros
- Declarative macros
- Procedural macros

### Declarative Macros
Most commonly used macros

