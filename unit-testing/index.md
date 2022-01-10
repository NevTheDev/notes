Moq
Bogus
XUnit

# Unit Testing

We don't mock the HttpClient, but we do mock the message handler that the HttpClient uses.

``` c#
var handler = new Mock<HttpMessageHandler>();
    handler
        .Protected()
        .Setup<Task<HttpResponseMessage>>(
            "SendAsync",
            ItExpr.IsAny<HttpRequestMessage>(),
            ItExpr.IsAny<CancellationToken>())
        .Returns(Task<HttpResponseMessage>.Factory.StartNew(returns)).Callback(cbAction);
```

## Mocking HttpClient

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
