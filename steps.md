```
// 


1. fork project and edit the same
https://github.com/atulkamble/azure-artifacts-python-helloworld#


clone code 

git clone https://github.com/username/azure-artifacts-python-helloworld.git
cd azure-artifacts-python-helloworld

2. open in vs code, terminal on 

3. open project https://dev.azure.com/atul-kamble/project

4. search artifacts - create feed 

create feed - python-packages (line 33)

5. after creating feed save your syntax details to .pypirc file

example: 

[distutils]
Index-servers =
  python-packages

[python-packages]
Repository = https://pkgs.dev.azure.com/atul-kamble/project/_packaging/python-packages/pypi/upload/

6. test manual command as well in vs code


python setup.py sdist bdist_wheel

7. git push repo code

https://github.com/username/azure-artifacts-python-helloworld#

git add .
git commit -m "code-update"
git push origin main

8. 

pipeline setting >> user >> build 

project build service, security service 

9. create token, note it down 


```
