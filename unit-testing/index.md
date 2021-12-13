# Code Coverage


## MSBUILD

``` cmd
dotnet add package coverlet.msbuild
``

```cmd
dotnet test /p:CollectCoverage=true
```


## Run in the console only

```cmd
dotnet tool install --global coverlet.console
```

```cmd
coverlet /path/to/test-assembly.dll --target "dotnet" --targetargs "test /path/to/test-project --no-build"
```