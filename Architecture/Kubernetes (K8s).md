A system that manages [[Docker| docker containers]].

**Horizontal Pod Autoscaling (HPA)**: The K8s feature that watches the CPU usage.
- *Scenario*: "If CPU usage goes above 70%, start 5 new containers."

**Self-Healing**: If a server crashes (Java Application exits), Kubernetes notices immediately and starts a fresh replacement.