# 💼 Best Practices for Structuring Service Classes in C# (.NET 8+)

> **Author:** 𝒩𝒶𝓊𝓂𝒶𝓃 𝐵𝒶𝓈𝒽𝒾𝓇

## 🧭 Purpose

A well-structured service class is foundational to building clean, maintainable, and scalable enterprise applications. This guide provides practical and professional conventions for organizing service logic using modern C# and .NET 8+, catering to junior developers, experienced engineers, and software architects alike.

Whether you're aiming for better code readability, improved testability, or a more decoupled system, these best practices will help you align with industry standards and architectural goals.


---

## 1. 🧩 Single Responsibility Principle (SRP)

**Do:** One responsibility per service class.

```csharp
public class EmployeeService : IEmployeeService
{
    public Task PromoteAsync(int employeeId) { /* ... */ }
}

public class PayrollService : IPayrollService
{
    public Task ProcessSalaryAsync(int employeeId) { /* ... */ }
}
```
**Don’t :**

```csharp
public class EmployeePayrollService 
{     
    public Task PromoteAsync(int employeeId) { /* ... */ }     
    public Task ProcessSalaryAsync(int employeeId) { /* ... */ } 
}
```

---

## 🔌 2. Interface-Driven Design

```csharp
public interface IEmployeeService 
{     
    Task<EmployeeDto> GetByIdAsync(int id); 
}
```

```csharp
public class EmployeeService : IEmployeeService 
{     
    // Implementation 
}
```

---

## 🧷 3. Dependency Injection

Use constructor injection. Don’t use `new`.

```csharp
public class EmployeeService : IEmployeeService 
{     
    private readonly IEmployeeRepository _repo;      

    public EmployeeService(IEmployeeRepository repo)     
    {         
        _repo = repo;     
    } 
}
```

---

## ⏳ 4. Asynchronous Programming

Use `async/await` with `Task` return types.

```csharp
public async Task<EmployeeDto> GetByIdAsync(int id) 
{     
    var entity = await _repo.FindAsync(id);     
    return MapToDto(entity); 
}
```

---

## 🪓 5. Keep Methods Small and Focused

```csharp
public async Task<EmployeeDto> GetByIdAsync(int id) 
{     
    var employee = await GetEmployeeAsync(id);     
    return MapToDto(employee); 
}
```

---

## 🔄 6. Encapsulate Mapping Logic

```csharp
private EmployeeDto MapToDto(Employee entity) 
{     
    return new EmployeeDto     
    {         
        Id = entity.Id,         
        Name = entity.Name     
    }; 
}
```

---

## 🛡️ 7. Validation and Guard Clauses

```csharp
public async Task<EmployeeDto> GetByIdAsync(int id) 
{     
    if (id <= 0)         
        throw new ArgumentException("Invalid ID");     
    // ... 
}
```

---

## ⚠️ 8. Exception Handling

Let exceptions bubble unless you can **log** or **wrap** them.

```csharp
try 
{     
    // call 
} 
catch (Exception ex) 
{     
    _logger.LogError(ex, "Something went wrong");     
    throw; 
}
```

---

## 🔒 9. Avoid Static State

Keep services stateless and avoid static fields unless constants.

---

## 🧵 10. Use Regions Sparingly

Only in large files. Prefer well-named methods.

```csharp
#region Mapping 
private EmployeeDto MapToDto(Employee entity) { ... } 
#endregion
```

---

## 📝 11. Documentation

Use XML comments for public classes/methods.

```csharp
/// <summary>
/// Fetches employee by ID.
/// </summary>
/// <param name="id">Employee ID</param>
/// <returns>Employee DTO</returns>
```

---

## 🗂️ 12. File-Scoped Namespaces (C# 10+)

```csharp
namespace MyApp.Services;

public class EmployeeService 
{ 
    // ...
}
```

---

## 🔤 13. Consistent Naming

Use verbs for methods: `CreateAsync`, `GetByIdAsync`, `UpdateAsync`.

---

## 📦 14. Return Types

Return DTOs, not EF entities.

```csharp
public async Task<EmployeeDto> GetByIdAsync(int id) 
{     
    var employee = await _repo.GetByIdAsync(id);     
    return _mapper.Map<EmployeeDto>(employee); 
}
```

---

## ✅ Summary

| Guideline              | ✅ Follow it? |
|------------------------|---------------|
| SRP                    | ✅ Yes        |
| Interface-first        | ✅ Yes        |
| Async & DI             | ✅ Yes        |
| Small, focused methods | ✅ Yes        |
| No statics, good docs  | ✅ Yes        |

---

