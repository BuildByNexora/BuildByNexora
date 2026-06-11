<div align="center">

# BuildByNexora

### Embedded infrastructure tools for Python and Rust.

I build small, durable systems that remove unnecessary infrastructure.

No Redis when a local runtime is enough.  
No broker when a Rust core can own the state.  
No cloud dependency when the application can carry the primitive itself.

[![Kron](https://img.shields.io/badge/Kron-persistent%20scheduling-blue)](https://github.com/BuildByNexora/Kron)
[![Flint](https://img.shields.io/badge/Flint-persistent%20rate%20limiting-orange)](https://github.com/BuildByNexora/Flint)
[![Python](https://img.shields.io/badge/Python-bindings-blue)](https://pypi.org/user/BuildByNexora/)
[![Rust](https://img.shields.io/badge/Rust-core-black)](https://github.com/BuildByNexora)

</div>

---

## What I Build

I am building an ecosystem of embedded infrastructure primitives:

| Project | What it does | Replaces |
|---|---|---|
| [Kron](https://github.com/BuildByNexora/Kron) | Persistent application scheduling | system cron, Celery beat, Redis-backed schedulers, cloud scheduler glue |
| [Flint](https://github.com/BuildByNexora/Flint) | Persistent embedded rate limiting | Redis rate limiters, in-memory counters, hand-written throttle tables |

The idea is simple:

```text
Redis-level simplicity
SQLite-level embeddability
domain-specific guarantees
```

---

## Kron

**Reliable application time, embedded first.**

Kron lets Python applications run durable scheduled jobs without a scheduler
server, Redis, RabbitMQ, Celery, or cloud scheduler.

```python
import kron

def sync_reports():
    ...

kron.schedule(
    "sync_reports",
    every="10m",
    fn=sync_reports,
    overlap="skip",
)

kron.start(data_dir=".kron")
```

What Kron gives you:

- persistent timers;
- run history;
- retries;
- crash recovery;
- local CLI inspection;
- single-writer data directory locking;
- overlap control for long-running jobs;
- Python bindings over a Rust core.

Install:

```bash
pip install kron-scheduler
```

Repository: [BuildByNexora/Kron](https://github.com/BuildByNexora/Kron)

---

## Flint

**Persistent rate limiting without Redis.**

Flint lets Python applications enforce rate limits with local durable state.
Counters survive restarts instead of resetting like normal in-memory limiters.

```python
import flint

limiter = flint.Limiter(data_dir=".flint")
limiter.limit("api:user-42", rate=100, per="1m")

if limiter.allow("api:user-42"):
    process_request()
```

What Flint gives you:

- token bucket, fixed window, and sliding window algorithms;
- persistent counters;
- snapshot and compaction;
- FastAPI middleware;
- shared mode for multiple local workers;
- Prometheus metrics export;
- Python bindings over a Rust core.

Install:

```bash
pip install flint-limiter
```

Repository: [BuildByNexora/Flint](https://github.com/BuildByNexora/Flint)

---

## Design Philosophy

Modern applications often add a full infrastructure component for a small
primitive:

- Redis just to run a rate limiter;
- Celery just to send a daily email;
- a cloud scheduler just to call a local endpoint;
- a custom database table just to delay work.

I prefer tools that are:

- **embedded**: run inside the application first;
- **durable**: keep state on disk;
- **observable**: expose status, history, metrics, and CLI inspection;
- **boring to operate**: no mandatory broker, daemon, or cloud dependency;
- **written close to the metal**: Rust core, simple Python API.

---

## Current Focus

```text
Kron   -> make application time reliable
Flint  -> make rate limits persistent
Next   -> more embedded infrastructure primitives
```

I am especially interested in tools for:

- edge applications;
- RISC-V and low-resource environments;
- Python backend services;
- local-first infrastructure;
- open-source developer tooling.

---

## Links

- [Kron on GitHub](https://github.com/BuildByNexora/Kron)
- [Flint on GitHub](https://github.com/BuildByNexora/Flint)
- [PyPI packages](https://pypi.org/user/BuildByNexora/)

