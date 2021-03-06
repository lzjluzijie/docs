---
name: Core Concepts
---

# Core Concepts

## Classic Macaron

To get up and running quickly, [`macaron.Classic`](https://gowalker.org/gopkg.in/macaron.v1#Classic) provides some reasonable defaults that work well for most of web applications:

```go
m := macaron.Classic()
// ... middleware and routing goes here
m.Run()
```

Below is some of the functionality [`macaron.Classic`](https://gowalker.org/gopkg.in/macaron.v1#Classic) pulls in automatically:

- Request/response logging - [`macaron.Logger`](../middlewares/core_services#routing-logger)
- Panic recovery - [`macaron.Recovery`](../middlewares/core_services#panic-recovery)
- Static file serving - [`macaron.Static`](../middlewares/core_services#static-files)

## Instances

Any object with type [`macaron.Macaron`](https://gowalker.org/gopkg.in/macaron.v1#Macaron) can be seen as an instance of Macaron, you can have as many instances as you'd like in a single piece of code.

## Handlers

Handlers are the heart and soul of Macaron. A handler is basically any kind of callable function:

```go
m.Get("/", func() string {
	return "hello world"
})
```

Non-anonymous function is also allowed for the purpose of using it in multiple routes:

```go
m.Get("/", myHandler)
m.Get("/hello", myHandler)

func myHandler() string {
	return "hello world"
}
```

Besides, one route can have as many as handlers you want to register with:

```go
m.Get("/", myHandler1, myHandler2)

func myHandler1() {
	// ... do something
}

func myHandler2() string {
	return "hello world"
}
```

### Return Values

If a handler returns something, Macaron will write the result to the current [`http.ResponseWriter`](http://gowalker.org/net/http#ResponseWriter) as a string:

```go
m.Get("/", func() string {
	return "hello world" // HTTP 200 : "hello world"
})

m.Get("/", func() *string {
	str := "hello world"
	return &str // HTTP 200 : "hello world"
})

m.Get("/", func() []byte {
    return []byte("hello world") // HTTP 200 : "hello world"
})

m.Get("/", func() error {
	// Nothing happens if returns nil
	return nil 
}, func() error {
	// ... get some error
	return err // HTTP 500 : <error message>
})
```

You can also optionally return a status code (only applys for `string` and `[]byte` types):

```go
m.Get("/", func() (int, string) {
	return 418, "i'm a teapot" // HTTP 418 : "i'm a teapot"
})

m.Get("/", func() (int, *string) {
	str := "i'm a teapot"
	return 418, &str // HTTP 418 : "i'm a teapot"
})

m.Get("/", func() (int, []byte) {
	return 418, []byte("i'm a teapot") // HTTP 418 : "i'm a teapot"
})
```

### Service Injection

Handlers are invoked via reflection. Macaron makes use of [Dependency Injection](http://en.wikipedia.org/wiki/Dependency_injection) to resolve dependencies in a Handlers argument list. **This makes Macaron completely  compatible with golang's [`http.HandlerFunc`](https://gowalker.org/net/http#HandlerFunc) interface.**

If you add an argument to your handler, Macaron will search its list of services and attempt to resolve the dependency via type assertion:

```go
m.Get("/", func(resp http.ResponseWriter, req *http.Request) {
	// resp and req are injected by Macaron
	resp.WriteHeader(200) // HTTP 200
})
```

The most commonly used service in your code should be [`*macaron.Context`](../middlewares/core_services#context):

```go
m.Get("/", func(ctx *macaron.Context) {
	ctx.Resp.WriteHeader(200) // HTTP 200
})
```

The following services are included with [`macaron.Classic`](https://gowalker.org/gopkg.in/macaron.v1#Classic):

- [`*macaron.Context`](../middlewares/core_services#context) - HTTP request context
- [`*log.Logger`](../middlewares/core_services#global-logger) - Global logger for Macaron instances
- [`http.ResponseWriter`](../middlewares/core_services#response-stream) - HTTP Response writer interface
- [`*http.Request`](../middlewares/core_services#request-object) - HTTP Request

### Middleware Handlers

Middleware Handlers sit between the incoming HTTP request and the router. In essence they are no different than any other Handler in Macaron. You can add a middleware handler to the stack like so:

```go
m.Use(func() {
  // do some middleware stuff
})
```

You can have full control over the middleware stack with the `Handlers` function. This will replace any handlers that have been previously set:

```go
m.Handlers(
	Middleware1,
	Middleware2,
	Middleware3,
)
```

Middleware Handlers work really well for things like logging, authorization, authentication, sessions, gzipping, error pages and any other operations that must happen before or after an HTTP request:

```go
// validate an api key
m.Use(func(ctx *macaron.Context) {
	if ctx.Req.Header.Get("X-API-KEY") != "secret123" {
		ctx.Resp.WriteHeader(http.StatusUnauthorized)
	}
})
```

## Macaron Env

Some Macaron handlers make use of the `macaron.Env` global variable to provide special functionality for development environments vs production environments. It is recommended that the `MACARON_ENV=production` environment variable to be set when deploying a Macaron server into a production environment.

## Handler Workflow

![](/docs/images/macaron_workflow.png)
