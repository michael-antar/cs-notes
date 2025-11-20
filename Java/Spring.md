# Spring

The industry standard for managing the lifecycle of objects through [dependency injection](../Dependency_Injection.md). It is an **open source library** maintained by Broadcom.

**See Also**:

- [Spring Boot](./Spring_Boot.md): An "opinionated" version of Spring, pre-configured with default settings.
- [Spring Cloud](./Spring_Cloud.md):

## Annotations

_Metadata tags_ used in Java code to configure the framework.

## Dependency Injection

At its core, Spring follows the _principle_ of **Inversion of Control (IoC)** (Instead of your code controlling the flow, the Framework controls the flow) through the _pattern_ of **Dependency Injection (DI)** (Injecting objects into constructors or variables).

```java
@RestController
public class ThermostatController {
    @Autowired
    private TemperatureService service;

    public void setTemp(int degrees) {
        service.updateHardware(degrees);
    }
}
```

## Components

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
