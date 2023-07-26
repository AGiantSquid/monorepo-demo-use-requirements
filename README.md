# monorepo-demo-install-requires

Repository demonstrating how to create a Python mono-repo that allows local dependencies to be installed in editable mode. This strategy requires the user to only ever install a project via its `requirements.txt` file. For example, to install `lib-a` a user would cd to the `lib-a` dir, and then do `pip install -r requirements.txt` instead of `pip install -e .`. Each `requirements.txt` file will link to the other `requirements.txt` file of every other local dependency.

# Demo

Install `lib-a` via its `requirements.txt`, which will in turn install `lib-b` and `lib-c` in editable mode via their respective `requirements.txt` files.

```
cd common-libraries/lib-a/
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

You can see that all 3 libs are installed like so:

```
python -c 'from lib_a import func_a ; func_a()'
python -c 'from lib_b import func_b ; func_b()'
python -c 'from lib_c import func_c ; func_c()'
```

This should print:

```
lib-a, func a
lib-b, func b
lib-c, func c
```

If you edit `lib-c`, its code update is reflected immediately in the venv.

```
echo "    print('edited')" >> ../lib-c/lib_c.py
```

Now check the lib-c code:
```
python -c 'from lib_c import func_c ; func_c()'
```

Should output:

```
lib-c, func c
edited
```

This demonstrates that the local dependencies are installed in editable mode in the venv.
