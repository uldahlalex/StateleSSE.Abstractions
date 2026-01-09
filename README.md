# StateleSSE.Abstractions

> **⚠️ DEPRECATED**: This package has been consolidated into `StateleSSE.AspNetCore`.
>
> **Migration:** Replace `StateleSSE.Abstractions` with `StateleSSE.AspNetCore` in your project. All types now exist in the `StateleSSE.AspNetCore` namespace.
>
> This package is no longer maintained and will not receive updates.

---

Core interface for building SSE backplane implementations.

## Installation

```bash
dotnet add package StateleSSE.Abstractions
```

## ISseBackplane Interface

```csharp
public interface ISseBackplane
{
    (ChannelReader<object> reader, Guid subscriberId) Subscribe(string groupId);
    void Unsubscribe(string groupId, Guid subscriberId);

    Task PublishToGroup(string groupId, object message);
    Task PublishToGroups(IEnumerable<string> groupIds, object message);
    Task PublishToAll(object message);

    int GetLocalSubscriberCount(string groupId);
    IEnumerable<string> GetLocalGroups();
    BackplaneDiagnostics GetDiagnostics();
}
```

## Available Implementations

| Package | Use Case |
|---------|----------|
| `StateleSSE.Backplane.InMemory` | Single-server, development, testing |
| `StateleSSE.Backplane.Redis` | Production horizontal scaling |

## Usage

```csharp
builder.Services.AddSingleton<ISseBackplane, MyBackplaneImplementation>();

[HttpGet("events")]
public async Task StreamEvents([FromQuery] string gameId)
{
    await HttpContext.StreamSseAsync<MyEvent>(backplane, $"game:{gameId}");
}
```

## License

MIT
