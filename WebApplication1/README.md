# WebApplication1

A .NET 10 minimal API with a sample Todo endpoint.

## Prerequisites

- .NET 10 SDK

## Run

From the `WebApplication1` project directory:

```powershell
dotnet run --launch-profile http
```

The app starts at `http://localhost:5244` in the Development environment, and the `http` profile opens a browser at `/todos`.

In Visual Studio, select the `http` profile and press F5.

## Test the API

### Endpoints

| Method | Path            | Description                                              |
| ------ | --------------- | -------------------------------------------------------- |
| GET    | `/todos`        | Returns all sample todos                                 |
| GET    | `/todos/{id}`   | Returns a single todo, or 404 if the id doesn't exist    |
| GET    | `/openapi/v1.json` | OpenAPI spec (Development environment only)           |

### curl / PowerShell

```powershell
curl http://localhost:5244/todos
curl http://localhost:5244/todos/1
curl -i http://localhost:5244/todos/99    # expect 404

Invoke-RestMethod http://localhost:5244/todos/2
```

### .http file

Open `WebApplication1.http` in Visual Studio or VS Code (with the REST Client extension) and click "Send Request" above a request.

## Known issue

`AppJsonSerializerContext` in `Program.cs` only registers `Todo[]`, not `Todo`. Because the app uses `CreateSlimBuilder` (no reflection-based serialization fallback), `GET /todos/{id}` may fail at runtime with a serialization error. To fix it, register both types:

```csharp
[JsonSerializable(typeof(Todo))]
[JsonSerializable(typeof(Todo[]))]
```
