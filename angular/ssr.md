# Angular - Server Side Rendering (SSR)

As with most javascript frameworks/platforms they like to control the whole flow of the application, this means that we very rairly create the raw html of a page and return that to the client.

This poses a few issues, mainly SEO impact.

```cmd
ng add @nguniversal/express-engine
```

Basically this causes angular to generate two versions of the application, one that runs on the server and one that is downloaded to the browser.

It also provides an express js webserver that will do the server side rendering.

## Issues and gotchas

The first issue I found was that when Angular loads in the browser it re-runs the HTTP Requests it run when rendering on the server. This is not ideal as we are waisting resources calling them from the server and then repeating these calls on the browser side.

Ideally we would like the browser side angular to not have to make another round of http requests.

One way to solve this is to update both the server and browser apps to read from a shared state, when angular makes the http requests on the server we can use a HttpInterceptor to capture the response and store in state.

The state is then transferred to the browser as part of the page, angular will load this into state.

We can then use another HttpInterceptor in the browser version of the application to check if an Http Request has already been made, and if so, we can load the response from the state.

### Server side interceptor

```typescript
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpResponse } from "@angular/common/http";
import { Inject, Injectable, Optional } from "@angular/core";
import { makeStateKey, TransferState } from "@angular/platform-browser";
import { Observable, tap } from "rxjs";
import { REQUEST } from '@nguniversal/express-engine/tokens'
import { Request } from 'express';

@Injectable()
export class ServerStateInterceptor implements HttpInterceptor  {

  constructor(
    @Optional() @Inject(REQUEST) protected request: Request,
    private transferState: TransferState) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req).pipe(tap(event =>
    {
      if(event instanceof HttpResponse && this.IsValidResponseCode(event)) {
        const protocolHost = `${this.request.protocol}://${this.request.get('host')}`;

        const stateKey = req.url.replace(protocolHost, '');

        const key = makeStateKey(stateKey);
        this.transferState.set(key, event.body);
        console.log(`Data pushed to Transfer State from server with Key: ${key}`);
      }
    }));
  }

  private IsValidResponseCode(event: HttpResponse<any>) : boolean {
    return event.status === 200 || event.status === 202;
  }
}
```

In the app.server.module.ts we need to import the ServerTransferStateModule and add the http interceptor to our providers

``` ts
import { ServerModule, ServerTransferStateModule } from '@angular/platform-server';
...
@NgModule({
  imports: [
    ...
    ServerTransferStateModule
  ],
  providers:[
    {
      provide: HTTP_INTERCEPTORS,
      useClass: ServerStateInterceptor,
      multi: true,
    }
  ...
```

### Browser side interceptor

Now we need to create the interceptor to check the state before making a request;

```ts
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpResponse } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { makeStateKey, TransferState } from "@angular/platform-browser";
import { Observable, of } from "rxjs";

@Injectable()
export class BrowserStateInterceptor implements HttpInterceptor
{
  constructor(private transferState: TransferState){
  }

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
   if(req.method === "GET"){
     const key = makeStateKey(req.url);
     const storedResponse = this.transferState.get<any>(key, null);
     if(storedResponse){
       this.transferState.remove(key);
       const response = new HttpResponse({body: storedResponse, status: 200});
       console.log(`Getting data from Transfer State for ${key}`);
       return of(response);
     }
   }

   return next.handle(req);
  }
}
```

We also need to update the NgModule Decorator in app.module.ts to pull in BrowserTransferStateModule and add the http interceptor.

```ts
...
import { BrowserModule, BrowserTransferStateModule } from '@angular/platform-browser';
...

@NgModule({
  ...
  imports: [
    ...
    BrowserTransferStateModule,
    ...
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: BrowserStateInterceptor,
      multi: true,
    }
    ...
```

## Proxy

We may also want to proxy http requests via the application server, this allows the adding of authentication keys without leaking these to the client.

Basically the angular app makes requests the the host server which then passes this request on to the correct API service.

```cmd
npm install express-http-proxy @types/express-http-proxy
```

### Proxy code

```ts
import * as proxy from "express-http-proxy";

export class APIProxy{
  constructor(
    private host: string,
     private replacement: string){}

  getProxy(){
    return proxy(this.host, {
      https:true,
      proxyReqPathResolver: (request) => {
        var parts = request.url.split('?');
        var path = parts[0];
        var newPath = path.replace(this.replacement, '');

        if (parts.length > 1) {
          newPath = `${newPath}?${parts[1]}`;
        }

        var rebuiltPath = `/${newPath}`

        console.log(rebuiltPath);
        return rebuiltPath;
      },
      proxyReqOptDecorator: (options: any, request) =>{
        options.rejectUnauthorized = false;

        return options;
      }
    })
  }
}

```

### Update the server.ts file

```ts
  server.get('/api/content/**', new APIProxy('https://localhost:7018/', '/api/content/').getProxy());
  server.get('/api/property/**', new APIProxy('https://localhost:7018/', '/api/property/').getProxy());
```

### Issues and Gotchas

When rendering on the server the app has issues resolving the base url of the project, we have to convert relative paths to absolute paths when making http request while running on the server.

```ts
import { HttpHandler, HttpInterceptor, HttpRequest } from "@angular/common/http";
import { Inject, Injectable, Optional } from "@angular/core";
import { REQUEST } from '@nguniversal/express-engine/tokens'
import { Request } from 'express';

const startsWithAny = (arr: string[] = []) => (value = '') => {
  return arr.some(test => value.toLowerCase().startsWith(test.toLowerCase()));
};

const isAbsoluteURL = startsWithAny(['http', 'https', '//']);

@Injectable()
export class RelativeToAbsolutePathInterceptor implements HttpInterceptor {

  constructor(@Optional() @Inject(REQUEST) protected request: Request){
  }

  intercept(req: HttpRequest<any>, next: HttpHandler) {

    if (this.request && !isAbsoluteURL(req.url)){

      const protocolHost = `${this.request.protocol}://${this.request.get('host')}`;

      const pathSeparator = !req.url.startsWith('/') ? '/' : '';
      const url = protocolHost + pathSeparator + req.url;
      const serverRequest = req.clone({ url });

      return next.handle(serverRequest);
    }

    return next.handle(req);
  }
}
```
