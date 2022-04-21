# Unit Testing

## Useful Packages

- Moq
- Bogus
- XUnit



## Expose Internals to Test Projects

```xml
<ItemGroup>
  <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
    <_Parameter1>$(AssemblyName).Tests</_Parameter1>
  </AssemblyAttribute>
</ItemGroup>
```

## Code Coverage

### MSBUILD

``` cmd
dotnet add package coverlet.msbuild
``

```cmd
dotnet test /p:CollectCoverage=true
```

### Run in the console only

```cmd
dotnet tool install --global coverlet.console
```

```cmd
coverlet /path/to/test-assembly.dll --target "dotnet" --targetargs "test /path/to/test-project --no-build"
```
