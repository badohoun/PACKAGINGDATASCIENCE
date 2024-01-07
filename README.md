# PACKAGINGDATASCIENCE

# Packaging terms:
- Module 
- Package
- sub-package
- distribution package 


# Modern way to build package :

* the old format distribution is sdist : source distribution package : python setup.py sdist 

   1. May make assumptions about customer machine:
      e.g. requires "gcc" to run "gcc numpy/*.c"
   2. Is slow: setup.py must be executed, compilation may be required.
   3. Is insecure: setup.py may contain  arbitrary code.
* the  good and new  format distribution is wheel 
   1. First install wheel package 
   2. the command help us to build wheel package : python setup.py bdist_wheel 
   3.  realpython.com/python-wheels/ explain clearly how to read wheel format 
   4.  we need to add build dependencies  for reproducibility and for it we need pyproject.toml file and install build tool : pip install build 
   5. escaping config hell; use setup.cfg : https://setuptools.pypa.io/en/latest/userguide/declarative_config.html
   6. Removing setup.cfg  thanks to pep 621 : peps.python.org/pep-0621/
   7. reconfigure project.toml ->  https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html (In setuptool doc Note New in 61.0.0)
   8. After you will see the information about your package in PKG-INFO inside of packaging-demo-egg-info
   9. Removing setup.py : PEP 517 ; build backends : peps.python.org/pep-0517/
   10. Exemple use case for including data with various formats (in this example is json) into your package 
   11. unzip your whl like this : unzip *.whl after you move to dist repo : cd dist and move to your repo and tape  pip install . 
   12. documentation to specific problems to add data files into your package : https://setuptools.pypa.io/en/latest/userguide/datafiles.html and https://setuptools.pypa.io/en/latest/userguide/miscellaneous.html#using-manifest-in
   13. python -m build --sdist --wheel ./; cd dist; unzip *.whl; cd ..
   14. for example to add data files recursively in your folder recursive-include packaging_demo/ *.json 
   15. equivalent of recursive include you can include in your pyproject.tom[tool.setuptools.package-data]
    my_pkg = ["*.json"] or you can use Hatch build system instead of setuptools : hatch.pypa.io/latest/config/build/#build-system poetry is another tool to build backend but is not pep 601 definition is not compliant 
 

 # Reproducibility 

 1. Dependency graph : add someting dependancies in pyproject.toml  and in terminal tape this command pip install -e.
2. visualize all dépendencies tree  of python libraries  use cli tool call  pipdeptree : pipdeptree --help , pipdeptree -p packaging-demo --grph-output png > dependency-graph.png , pip install graphviz  , and brew install graphviz or sudo apt-get install graphviz help us to the issues dependencies hell : constraint-trees (datascientist didn't document version of the dependencies we use ) the more library  they more constraints you have you had they more dependencies you have lot of constraints 
3. Document all of the exact version of library and the exact  python version and in addition of that you documented  opération system : m series mcbook , linus ..etc  the solution to this is  if you run pip freeze  : this is most vanilla way in python to freeze or lock your dependencies list : set of the exact version that satisfied constraint pip freeze > requirements.txt useful for developpment reproducibilty  poetry , pip-tools same willl help you .
4. you can add [project.optional-dependencies]
dev = ["ruff" , "mypy" , "black"] and after that run in terminal pip install '.[dev]' the same for rich add colors = ["rich] pip install '.[rich]' or pip install '.[colors,dev]' or  all = ["packaging_demo[dev , colors]"] and pip install '.[all]' package index search : snyk.io/advisor/python/package-index ( give you criteria to check package health in terms of security maintenance , community )