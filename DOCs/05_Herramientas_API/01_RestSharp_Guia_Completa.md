# RestSharp - Guía Completa para API Testing

## Instalación
```bash
dotnet add package RestSharp
dotnet add package RestSharp.Serializers.NewtonsoftJson
dotnet add package xunit
dotnet add package Microsoft.AspNetCore.Mvc.Testing
```

## Configuración Básica
```csharp
var client = new RestClient("https://api.example.com");
client.Authenticator = new JwtAuthenticator(token);
```

## Patrones de Test
```csharp
[Fact]
public async Task GetUser_ReturnsOk()
{
    var request = new RestRequest("/api/users/1", Method.Get);
    var response = await client.ExecuteAsync<User>(request);
    
    Assert.Equal(HttpStatusCode.OK, response.StatusCode);
    Assert.NotNull(response.Data);
    Assert.Equal(1, response.Data.Id);
}
```

## Autenticación
- JWT Bearer Token
- OAuth 2.0
- API Key
- Basic Auth

## Data-Driven Tests
```csharp
[Theory]
[InlineData("admin", 200)]
[InlineData("user", 200)]
[InlineData("invalid", 401)]
public async Task Login_ReturnsExpectedStatus(string user, int expected)
{
    // ...
}
```

## Integración CI/CD
- GitHub Actions
- Azure DevOps
- Jenkins