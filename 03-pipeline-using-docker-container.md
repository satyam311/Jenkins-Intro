# Pipeline using Docker container as worker node


<img width="1110" height="562" alt="Screenshot 2026-01-01 at 12 01 16 PM" src="https://github.com/user-attachments/assets/93a6884b-1fe5-451a-9c97-65022c42217c" />

### Jenkins with EC2 Workers (Traditional Setup)

**What’s happening:**

- Jenkins Master is running on one EC2 instance.
- It connects to multiple worker nodes, each running on separate EC2 instances.
- Each worker is a full virtual machine.

**How it works:**

- Jenkins master schedules jobs.
- Jobs are executed on separate EC2 worker machines.
- Each worker has:
- Its own OS
- Its own CPU & memory
- Tools installed (Java, Docker, Maven, etc.)

**Pros:**

✅ Strong isolation
✅ Stable and persistent
✅ Easy to understand

**Cons:**

❌ Expensive (each worker = full EC2 instance)
❌ Slower to scale
❌ Resource heavy

### Jenkins with Containerized Workers


**What’s happening:**

- Jenkins Master runs on one EC2 instance.
- Workers are containers, not full machines.
- Containers run on the same host (or Kubernetes node).

**How it works:**

- Jenkins dynamically spins up containers as workers.
- Each container runs a job and then can be destroyed.
- Often implemented using:
  - Docker
  - Kubernetes
  - ECS / EKS

**Pros:**

✅ Fast startup
✅ Lightweight
✅ Cost-effective
✅ Easy scaling
✅ Clean environment per job

**Cons:**

❌ Slightly more complex setup
❌ Requires container knowledge
