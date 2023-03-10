
# pip

Python packages can be installed easily using **pip**. By default, **pip** will attempt to install packages to a system location, but Roar and RC are shared systems. Users do not have write access to system locations on shared systems. The packages can instead be installed in *~/.local* which is a user location using the following:
```
$ pip install --user \<package>
```

