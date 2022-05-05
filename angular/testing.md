# Angular Testing

## Testing Services that use HttpClient

Import the HttpTestingController and HttpClientTestingModule this allows us to replace the httpClient with a testable version.

```ts
import { HttpTestingController, HttpClientTestingModule,} from '@angular/common/http/testing';
```

Update the TestBed to import the HttpClientTestingModule

```ts
TestBed.configureTestingModule({
    imports: [
        HttpClientTestingModule
    ],
});
```

We can now ask the TestBed for an instance of the HttpController to use in our test.

```ts
let httpMock: HttpTestingController;

...

beforeEach(() => {

    ...

    httpMock = TestBed.inject(HttpTestingController);
})

```

We should call verify to make sure that there are no outstanding unmatched http requests.

To make sure this happens after each test we will add a after each step

```ts
afterEach(() => {
    httpMock.verify();
});
```

### Checking the correct behaviour

The http test controller exposes a number of methods we can use to check what actions the http client made.

```ts
const request = httpMock.expectOne(expectedUrl)

// check the Method of the http request
expect(request.request.method).toBe('GET');

```

As we create our service via the TestBed the HttpClient