# Web Dependency Reachability Project

## Project Overview

This project investigates whether vulnerable npm dependencies are actually used and reachable through web applications.

Traditional Software Composition Analysis (SCA) tools, such as npm audit and OSV-Scanner, identify known vulnerabilities in installed dependencies. However, these tools do not necessarily determine whether an application executes the vulnerable code or whether it can be reached through an HTTP request.

Our goal is to compare traditional vulnerability scanning with dependency usage and web reachability analysis.

## Benchmark Cases

The project consists of four Node.js/Express web applications, each representing a different dependency scenario.

| Case | Classification | Description |
|---|---|---|
| C01 | SAFE | Dependency is patched or not affected by the selected vulnerability. |
| C02 | PRESENT_UNUSED | Vulnerable dependency is installed but never imported or called. |
| C03 | USED_NOT_WEB_REACHABLE | Vulnerable dependency is used internally but cannot be reached through an HTTP route. |
| C04 | WEB_REACHABLE | An HTTP request reaches the relevant vulnerable dependency function. |

## Repository Structure

```text
web_dependency_reachability_project/
├── benchmarks/
│   ├── C01-safe/
│   ├── C02-present-unused/
│   ├── C03-internal-only/
│   └── C04-web-reachable/
├── scanner-results/
├── ground-truth/
├── results/
├── docs/
│   └── evidence/
├── verifier/
├── scripts/
├── compose.yaml
└── README.md
```

### Directory Descriptions

- **benchmarks/** — Contains the four benchmark web applications.
- **scanner-results/** — Stores raw vulnerability scanner outputs.
- **ground-truth/** — Contains the expected classifications for each benchmark.
- **results/** — Stores dependency analysis, scanner comparisons, and reachability results.
- **docs/evidence/** — Contains HTTP responses, execution logs, and source-code mapping evidence.
- **verifier/** — Contains the prototype dependency reachability verifier.
- **scripts/** — Contains shared scripts for scanning and testing.
- **compose.yaml** — Defines the Docker services used to run the benchmark applications.

## Requirements

- Docker
- Docker Compose
- Git
- Node.js and npm (for running applications outside Docker)

## Running the Benchmarks

### 1. Clone the Repository

```bash
git clone https://github.com/Hx-hd/web_dependency_reachability.git
cd web_dependency_reachability
```

### 2. Start an Application

Each benchmark application can be started independently using Docker Compose.

For example, to start C02:

```bash
docker compose up -d --build c02
```

### 3. Test the Application

Send a request to the application's HTTP endpoint:

```bash
curl "http://localhost:3002/api/demo?name=[anything]"
```

### 4. View Container Logs

```bash
docker compose logs c02
```

### 5. Stop the Containers

```bash
docker compose down
```

## Methodology

The project follows these steps:

1. Build the benchmark web applications.
2. Select dependency and vulnerability pairs.
3. Configure the four benchmark scenarios.
4. Run vulnerability scanners.
5. Compare scanner findings.
6. Verify dependency presence and installed versions.
7. Determine whether dependencies are imported and called.
8. Map HTTP routes to dependency functions.
9. Assign contextual vulnerability classifications.
10. Develop and evaluate a prototype verifier.

## Vulnerability Scanners

The project uses two Software Composition Analysis tools:

- **npm audit**
- **OSV-Scanner**

Scanner outputs are stored in `scanner-results/` and compared with the contextual findings recorded in `results/`.

## Final Classifications

The verifier uses the following contextual labels:

| Classification | Meaning |
|---|---|
| SAFE | Dependency is patched or not affected. |
| PRESENT_UNUSED | Vulnerable dependency is installed but unused. |
| USED_NOT_WEB_REACHABLE | Dependency is used internally but not reachable through HTTP. |
| WEB_REACHABLE | HTTP requests reach the relevant dependency function. |
| UNKNOWN | Insufficient evidence to determine reachability. |

## Project Goal

The goal is to demonstrate how dependency usage and HTTP reachability analysis can help prioritize vulnerability findings beyond traditional dependency scanning.

Web reachability does not necessarily imply that a vulnerability is exploitable.
