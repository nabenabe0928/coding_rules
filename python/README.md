# Goal of coding rules
The main goals of the coding rules are to:
1. increase the productivity and decrease the maintenance cost
2. decrease the number of potential bugs
3. increase the interpretability of the codes

# Rule 1. Use autopep8, flake8, mypy, google style docstring
[autopep8](https://www.python.org/dev/peps/pep-0008/)
and [flake8](https://flake8.pycqa.org/en/latest/)
are format rules and when you put the following code in `.vscode/settings.json`,
vscode can detect the violation in your codes.
This file is provided in this repository as well.

```
{
    "python.linting.pylintEnabled": false,
    "python.linting.pycodestyleEnabled": true,
    "python.linting.enabled": true,
    "python.linting.flake8Enabled": true,
    "python.formatting.provider": "autopep8",
    "python.linting.pycodestyleArgs":[
        "--max-line-length=120",
    ],
    "python.linting.flake8Args":[
        "--max-line-length=120",
    ],
    "python.formatting.autopep8Args": [
        "--aggressive", "--aggressive",
    ],
    "python.pythonPath": "<Your python path>",
}
```
For autopep8, we especially would like you to follow the rules described in
[Naming Conventions](https://www.python.org/dev/peps/pep-0008/#naming-conventions),
[Comments](https://www.python.org/dev/peps/pep-0008/#comments).

mypy is an analysis tool for python typing.
For example, when you define a function:
```
def additive(a: int, b: int) -> int:
    return a + b
```
mypy can analyze if the code satisfies the typing.

google styple docstring is used for the documentation of codes.
For more details, refer to [section 3.8 Comments and Docstrings in pyguide](https://google.github.io/styleguide/pyguide.html#s3.8.1-comments-in-doc-strings).
We also provide the `template.code-snippets` in `.vscode/` directory.

# Rule 2. Don't use Any for typing
- use class or `NamedTuple` instead of `Dict[str, Any]`

```
class Father():
    ...


class Mother():
    ...


class Children():
    ...

family = {'papa': Father(...), 'mama': Mother(...), 'me': Children(...)}

class Family():
    __slots__ ('papa', 'mama', 'me')

    def __init__(self, papa, mama, me):
        self.papa = papa
        self.mama = mama
        self.me = me


from typing import NamedTuple


class Family(NamedTuple):
    papa: Father
    mama: Mother
    me: Children
```

# Rule 3. Narrow variable's scope

# Rule 4. Do not hold the same variables in different locations

# Rule 5. Name variables properly 

# Rule 6. Don't nest too much

# Rule 7. Don't make classes and functions large
- make class small and do not inherit too much

# Rule 8. Use unittests

# Rule 9. Don't use magical numbers or strings

# Rule 10. Don't code same processing multiple times

# Rule 11. Make variables private

[My note](https://github.com/automl/Auto-PyTorch/issues/148)

[Google Style python coding](https://google.github.io/styleguide/pyguide.html)
