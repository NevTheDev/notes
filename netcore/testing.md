# Testing

## Expose Internal classes to test project

``` c#
[assembly: InternalsVisibleTo("name_of_test_assembly")]
```

``` xml
<ItemGroup>
  <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
    <_Parameter1>name_of_test_assembly</_Parameter1>
  </AssemblyAttribute>
</ItemGroup>
```
