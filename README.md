# Test Repo to Recreate force-include issue in Hatchling v1.19.0+
https://github.com/pypa/hatch/issues/1130

# Instructions to recreate issue
```console
git clone git@github.com:hguturu/hatching_test.git

cd hatching_test

# to re-create the issue
tox

# issue caused due to tox installing from sdist, to recreate directly
python -m build . --sdist
pip install dist/footest-1.0.tar.gz

# the  following works
pip install .
> python -m footest
# importing footest
> ls ~/miniconda3/lib/python3.10/site-packages/footest
FooSans  __init__.py  __pycache__  stylelib

# the following works (dev install)
pip install -e .
> python -m footest
# importing footest
> ls ~/miniconda3/lib/python3.10/site-packages/footest
FooSans  stylelib

# if workaround of removing the follow is used, then the `pip install .` doesn't work anymore
[tool.hatch.build.targets.wheel.force-include]
"external/stylelib" = "src/foo/stylelib"

pip install .
> python -m footest
# importing footest
> ls ~/miniconda3/lib/python3.10/site-packages/footest
__init__.py  __pycache__
# the force included folders are missing
```
