# Black formatting
Black not working because of (most probably) an incompatible version of click (click-7.1.2) but VSCode doesn't show any error in the "Python" logs

Running the same black command that vscode users in a terminal:
```
❯ ~/workspace/pravda/venv/bin/python -m black --diff --quiet ./tests/test_app_docker.py
Traceback (most recent call last):
  File "/usr/lib/python3.9/runpy.py", line 188, in _run_module_as_main
    mod_name, mod_spec, code = _get_module_details(mod_name, _Error)
  File "/usr/lib/python3.9/runpy.py", line 147, in _get_module_details
    return _get_module_details(pkg_main_name, error)
  File "/usr/lib/python3.9/runpy.py", line 111, in _get_module_details
    __import__(pkg_name)
  File "src/black/__init__.py", line 35, in <module>
ImportError: cannot import name 'ParameterSource' from 'click.core' (/home/mohammedi/workspace/pravda/venv/lib/python3.9/site-packages/click/core.py)
```
