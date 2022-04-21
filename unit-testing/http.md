# Testing http

We may want to test code we have written around the http client, for this we will need to be able to mock the http requests and responses.

We should consider if the tests we are writing are really needed as this kind of testing can result in lots of test setup code and maintenance overhead.

## Mocking HttpClient

We don't mock the HttpClient, but we do mock the HttpMessageHandler that the HttpClient uses.

``` c#
var response = new HttpResponseMessage();
var handler = new Mock<HttpMessageHandler>();
    handler
        .Protected()
        .Setup<Task<HttpResponseMessage>>(
            "SendAsync",
            ItExpr.IsAny<HttpRequestMessage>(),
            ItExpr.IsAny<CancellationToken>())
        .ReturnsAsync(response);

return new HttpClient(handler.Object)
{
    BaseAddress = new Uri("https://identity.test.example.com"),
};

```

## Testing Delegating Handlers

```c#
// Add a Correlation ID to all outgoing requests
public class CorrelationHandler : DelegatingHandler
{
    private ICorrelationIdProvider _provider;

    public CorrelationHandler(ICorrelationIdProvider provider)
    {
        _provider = provider;
    }

    protected async override Task<HttpResponseMessage> SendAsync(
            HttpRequestMessage request,
            CancellationToken cancellationToken)
        {
            var correlationId = _provider.Get();
            request.Headers.Add("x-correlation", correlationId);

            return await base.SendAsync(request, cancellationToken);

        }
}

// Class is used to mock the low level handler on httpclient.
public class DelegatingHandlerTestHandler : DelegatingHandler
{
    protected override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request,
        CancellationToken cancellationToken)
    {
        return Task.FromResult(new HttpResponseMessage(HttpStatusCode.OK));
    }
}

[Fact]
public async Task AddsCorrelationIdToOutboundRequests()
{
    var innerHandler = new DelegatingHandlerTestHandler();
    var handler = new BearerTokenAuthenticationHandler(mockBearerTokenProvider.Object)
    {
        InnerHandler = innerHandler,
    };

    var invoker = new HttpMessageInvoker(handler);

    var httpRequest = new HttpRequestMessage(
        HttpMethod.Get,
        "/hello/world/");

    await invoker.SendAsync(httpRequest, default);

    var values = httpRequest?.Headers?.GetValues("x-correlation");
    ...
}


```