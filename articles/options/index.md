# Excepticon Options

Excepticon behavior can be configured via options specified in your application configuration provider, or by setting them direct on the `ExcepticonOptions` object registered with your DI container.

### ApiKey
The API key that identifies your project to Excepticon.  Required.
```
  "Excepticon": {
    "ApiKey": "{Your ApiKey Here}",
  }
```

### ExcludedExceptionTypes
A semicolon-delimited list of names of exception types that should be excluded from being sent to Excepticon.  Type names can either be the simple type name or the fully-qualified type name.
```
  "Excepticon": {
    "ApiKey": "{Your ApiKey Here}",
    "ExcludedExceptionTypes": "UnauthorizedException;BadRequestException"
  } 
```