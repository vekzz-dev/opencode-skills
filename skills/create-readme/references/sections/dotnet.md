# C# /.NET Project README Template

Use this template for .NET projects (libraries, web apps, console apps).

## Structure

```markdown
# <project-name>

One-line description of what this .NET project does.

[![NuGet][nuget-badge]][nuget-url]
[![.NET][dotnet-badge]][dotnet-url]
[![License][license-badge]][license-url]

## Why?

Explain the problem this project solves.

## Installation

### NuGet Package

```bash
dotnet add package Project.Name
```

```powershell
Install-Package Project.Name
```

### Package Reference

```xml
<PackageReference Include="Project.Name" Version="1.0.0" />
```

## Quick Start

```csharp
using Project.Name;

var client = new Client();
var result = client.DoSomething("input");
Console.WriteLine(result);
```

## Usage

### Console App

```csharp
// Program.cs
var result = await ProjectName.ProcessAsync(input);
Console.WriteLine(result);
```

### Web API

```csharp
[ApiController]
[Route("api/[controller]")]
public class ItemsController : ControllerBase
{
    [HttpGet("{id}")]
    public async Task<Item> Get(int id)
    {
        return await _service.GetItemAsync(id);
    }
}
```

### Library

```csharp
using ProjectName;

var config = new Config { Option = true };
var client = new Client(config);
var result = client.Process(input);
```

## Configuration

### appsettings.json

```json
{
  "ProjectName": {
    "Option": true,
    "ApiKey": "${API_KEY}"
  }
}
```

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `ProjectName__ApiKey` | API key | - |
| `ProjectName__Debug` | Enable debug | `false` |

### Code Configuration

```csharp
var config = new Config
{
    Option = true,
    ApiKey = "your-key"
};
```

## API Reference

### Classes

```csharp
public class Client
{
    public Client(Config config)
    public Task<string> ProcessAsync(string input)
    public void Dispose()
}
```

### Config

```csharp
public class Config
{
    public bool Option { get; set; }
    public string ApiKey { get; set; }
}
```

## Examples

### Basic Usage

```csharp
using ProjectName;

var client = new Client();
var result = await client.ProcessAsync("input");
Console.WriteLine(result);
```

### With Dependency Injection

```csharp
// Program.cs
builder.Services.AddSingleton<IClient, Client>();

// Controller
public class MyController : ControllerBase
{
    private readonly IClient _client;
    
    public MyController(IClient client) => _client = client;
}
```

## Requirements

- .NET 8.0+
- Visual Studio 2022 or VS Code

## Building

```bash
# Build
dotnet build

# Run
dotnet run

# Test
dotnet test

# Publish
dotnet publish -c Release
```

## NuGet Package

```bash
# Pack
dotnet pack

# Push to NuGet
dotnet nuget push Project.Name.nupkg --source https://api.nuget.org/v3/index.json
```

## Testing

```bash
# Run tests
dotnet test

# With coverage
dotnet test --collect:"XPlat Code Coverage"

# Specific project
dotnet test tests/Project.Name.Tests
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)
```

---

## .NET-Specific Elements

1. **.csproj** - Project file
2. **NuGet** - Package manager
3. **dotnet add package** - Installation command
4. **.NET detection** in project-types
5. **appsettings.json** - Configuration
6. **Dependency Injection** - Common pattern
7. **dotnet CLI** - Build/run/test commands
8. **MSBuild** - Build system
9. **xUnit/NUnit/MSTest** - Testing frameworks
