Practice architectural scenarios and how to design backends.

## "Hot Path" vs. "Cold Path"

A device sends a heartbeat `{"id": 101, "temp": 25.5}`.

---

1. **Gateway**: Receives the TCP packet and pushes the JSON to Kafka Topic `telemetry`.
2. **Spring Boot Consumer**: Pulls the message from Kafka
3. **Branching Logic**:
   - **Hot Path (Real-time)**: The service writes the value `25.5` to Redis Key `device:101:current`. This creates the "live view" for the mobile app.
   - **Cold Path (Historical)**: The service writes the value `25.5` + `timestamp` to a Time-Series Database (or MySQL). This allows the user to view a graph of their temperature over the last month.

Here is a Spring Boot example:

```java
@Service
public class TelemetryProcessor {

    // REDIS CONNECTION
    // Spring injects the connection to the Redis Cluster
    @Autowired
    private StringRedisTemplate redis;
	
    // DATABASE CONNECTON
    @Autowired
    private TelemetryRepository dbRepository;
	
    // KAFKA LISTENER (The Entry Point)
    // This method is triggered automatically when Kafka has a new message
    @KafkaListener(topics = "device-telemetry", groupId = "backend-groups-1")
    public void processMessage(String jsonMessage) {
		
        // Parse the JSON
        TelemetryData data = parse(jsonMessage);
		
        // --- HOT PATH (REDIS) ---
        // Overwrite the current state so the dashboard is instant.
        // O(1) operation.
        redis.opsForValue().set("device:" + data.getId(), data.getTemp());
		
        // --- COLD PATH (DATABASE) ---
        // Save to history for future graphs.
        // Slower operation, but doesn't block the user UI.
        dbRepository.save(data);
    }
}
```
