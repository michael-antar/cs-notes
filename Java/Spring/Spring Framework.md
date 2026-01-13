Open-source [[Java]] framework for enterprise applications.

**See Also**:
- [[Spring Boot]]: An *"opinionated"* version of Spring that comes *pre-configured* with packages.
- [[Spring Cloud]]: A collection of tools (Gateway, Circuit Breaker, Config Server) to manage communication between **microservices**.
## Annotations
_Metadata tags_ (`@Symbol`) that tell Spring how to treat a class or method.
### Structural Annotations
These tell Spring *"Create an instance of this class and manage it"*

- `@Component`: The generic "catch-all" annotation. Any class with this becomes a **bean**
- `@Service`: A specialized version of `@Component` for **business logic**
	- Use this layer for _calculations, validation, and calling other APIs_
- `@Repository`: A specialized version of `@Component` for **database access**
	- It catches database-specific errors (like SQL exceptions) and translates them into clean Spring exceptions
- `@RestController`: A specialized version of `@Component` for **web APIs**
	- It combines `@Controller` (Web) + `@ResponseBody` (JSON return type)
- `@Configuration`: Defines a class that is a source of bean definitions (used to setup 3rd party libraries)
### Behavioral Annotations
These tell Spring *"Do something specific to this existing object"*

- `@Autowired`: Tells Spring to find a Bean (Dependency) and inject it here
- `@Transactional`: Wraps a method in a "Database Transaction". If the method fails, all database changes inside it are **rolled back**
- `@Bean`: Used _inside_ a `@Configuration` class. It tells Spring, "Take the return value of this method and manage it as a Bean"
	 - _Use case_: When you need to use a class from an external library (like Gson) and can't add `@Component` to its source code

## Components & Beans
A **bean** is simply a Java Object that is instantiated, assembled, and managed by the **Spring IoC Container**
- *Default Scope*: **Singleton**. Spring creates *ONLY ONE* instance of the object and reuses it for the entire application.

The **component** is the "engine" of Spring. It scans your code for `@Component`, creates the Beans, and wires them together using *Dependency Injection*.
### Dependency Injection
At its core, Spring follows the _principle_ of **Inversion of Control (IoC)** (Instead of your code controlling the flow, the Framework controls the flow) through the _pattern_ of **Dependency Injection (DI)** (Injecting objects into constructors or variables).

```java
@Service
public class ThermostatService {
    @Autowired
    private TemperatureService service;
	
    public void setTemp(int degrees) {
        service.updateHardware(degrees);
    }
}
```

## MVC Architecture
Spring uses the MVC architecture to build APIs:

- **Model**: Beans and Entities
	- Lives in the Database and in the Service layer.
- **View**: JSON data
	- Simply the text (e.g., `{ "status": "ON", "temp": 72 }`)
- **Controller**: `@RestController`. Handles the incoming HTTP requests.
	- It listens for a request (e.g., `GET /devices`), calls the Service layer to get the data, and returns the result

```java
// --- THE CONTROLLER (C) ---
@RestController
public class DeviceController {
	
    @Autowired
    private DeviceService service;
	
    @GetMapping("/device/{id}")
    public Device getDevice(@PathVariable String id) {
        return service.findDevice(id);
    }
}

// --- THE MODEL (M) ---
public class Device {
    private String id;
    private String status;
    // Getters and Setters...
}

// --- THE VIEW (V) ---
/* There is no Java file for this in a REST API.
Spring automatically converts the 'Device' object into this JSON "View":
{
    "id": "123",
    "status: "ONLINE"
}
*/
```
