# Python Coding Standards for Phincon's AI Platform

This document outlines the official Python coding standards for Phincon's AI platform project. Adherence to these guidelines is mandatory to ensure a consistent, readable, and maintainable codebase.

## 1. Guiding Principles

-   **Readability & Consistency:** Code is read more often than it is written. Our primary goal is a clean, consistent style across the entire project.
    
-   **Pragmatism Over Dogma:** Know when to break a rule. If a deviation from this guide improves clarity or performance, it is permissible, but it must be justified with a comment.
    
-   **Automation:** We automate formatting and linting to eliminate style debates and focus code reviews on logic and architecture.
    

## 2. The Automated Toolchain

Our code quality is enforced by a mandatory toolchain. All code must pass checks from these tools before being merged.

-   **Formatter: `Black`**: The uncompromising code formatter. All code will be formatted with `Black` using its default settings.
    
-   **Linter & Import Sorter: `Ruff`**: An extremely fast linter that replaces Flake8, isort, and many other plugins. It handles error checking and import sorting.
    

### `pyproject.toml` Configuration

This configuration is the single source of truth and must be present in the project root.

~~~Toml
[tool.black]
line-length = 88
target-version = ['py311']

[tool.ruff]
line-length = 88
target-version = "py311"

[tool.ruff.lint]
# See Ruff docs for a full list of rule codes to enable.
# Example: Pyflakes, pycodestyle, isort, pep8-naming, etc.
select = ["E4", "E7", "E9", "F", "B"]
# Ignore line length violations, as Black handles them.
ignore = ["E501"]

[tool.ruff.lint.isort]
# Define our main application package as first-party.
known-first-party = ["app"]
~~~

## 3. Code Layout

-   **Imports**:
    
    -   Use **absolute imports** only (e.g., `from app.core import settings`). Relative imports are forbidden.
        
    -   Never use wildcard imports (`from module import *`).
        
    -   `Ruff` will automatically group and sort imports into three sections: standard library, third-party, and first-party.
        
-   **Line Length**: Maximum line length is **88 characters**, enforced by `Black`.
    
-   **Indentation**: Use **4 spaces** for indentation. No tabs.
    
-   **Whitespace**:
    
    -   Surround binary operators with a single space (e.g., `x = y + 1`).
        
    -   Use two blank lines between top-level functions and classes, and one blank line between methods in a class.
        

## 4. Naming Conventions

Names must be descriptive and unambiguous. We follow PEP 8 standards.

| Identifier Type | Casing Style | Example |
| ----------- | ----------- | ----------- |
| Module / Package | `lower_with_under` | `db_utils.py` |
| Class / Exception | `CapWords` | `DatabaseSession` |
| Function / Method | `lower_with_under` | `get_user_by_id` |
| Variable | `lower_with_under` | `current_user` |
| Constant | `UPPER_WITH_UNDER` | `MAX_RETRIES = 3` |

-   **Internal Use**: Prefix internal-use-only functions, methods, or attributes with a single underscore (e.g., `_internal_helper`). This signals it is not part of the public API.
    
-   **Name Mangling**: The use of double leading underscores (`__`) is forbidden to ensure testability and debuggability.
    

## 5. Documentation and Comments

-   **Comments**: Explain the **"why,"** not the "what." Good code is self-documenting; comments should clarify complex logic or business reasons.
    
-   **Docstrings**: All public modules, functions, classes, and methods **must** have docstrings formatted in the **Google Python Style**. This is essential for our auto-generated API documentation.
    

### Google Style Docstring Example

~~~Python
def calculate_risk_score(user_profile: dict, transaction_history: list[dict]) -> float:
    """Calculates a risk score for a user.

    Args:
        user_profile (dict): A dictionary containing user demographic data.
        transaction_history (list[dict]): A list of the user's past transactions.

    Returns:
        float: The calculated risk score, a value between 0.0 and 1.0.

    Raises:
        ValueError: If the transaction_history is empty.
    """
    if not transaction_history:
        raise ValueError("Transaction history cannot be empty.")
    #... implementation...
    return 0.75
~~~

## 6. Modern Python Best Practices

-   **Type Hinting**:
    
    -   All new function and method signatures **must** be fully type-hinted.
        
    -   Use modern syntax: `str | int` instead of `Union[str, int]`.
        
    -   Use `list[str]` instead of `typing.List[str]`.
        
    -   Use `str | None` for optional types, with `None` last.
        
-   **Idiomatic Comparisons**:
    
    -   **Booleans**: Use `if is_active:` not `if is_active == True:`.
        
    -   **None**: Use `if user is not None:` not `if user!= None:`.
        
    -   **Empty Collections**: Use `if user_list:` not `if len(user_list) > 0:`.
        
-   **Context Managers**: Use the `with` statement for managing resources like files or database connections to ensure they are properly closed.
