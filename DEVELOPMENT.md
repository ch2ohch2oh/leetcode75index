# Development Guidelines

This project requires a specific Python environment setup.

## Virtual Environment (venv)

**CRITICAL:** This project uses a local virtual environment named `venv`.

*   **Always** use the python executable from the virtual environment.
*   **Do not** use the system global python.

### Running Scripts

You have two options:

1.  **Activate the environment first (Recommended for shells):**
    ```bash
    source venv/bin/activate
    python collect_stats.py
    ```

2.  **Use the direct path:**
    ```bash
    ./venv/bin/python collect_stats.py
    ```

## Dependencies

*   Install dependencies into the `venv`:
    ```bash
    source venv/bin/activate
    pip install -r requirements.txt
    ```
*   If you add new packages, update `requirements.txt`:
    ```bash
    pip freeze > requirements.txt
    ```
