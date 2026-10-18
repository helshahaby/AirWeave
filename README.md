# AIRWEAVE --- The Air Picture That Knows When It's Wrong

**Project owner:** Hossam Elshahaby\
**Application:** [Link](https://airweave-sensor-fusion.lovable.app)
**Demo:** [Link](https://youtu.be/DpOcBFtv4kA)
**Tagline:** One shared picture. Multiple imperfect sensors. Honest
uncertainty.

AIRWEAVE is a proposed synthetic-data research demonstrator for
combining imperfect observations from multiple low-cost sensor nodes
into a common operational picture. Its core differentiator is explicit
uncertainty and data-quality awareness: the system should expose stale
reports, sensor outages, ambiguous associations, and degraded
communications instead of presenting every estimate as equally
trustworthy.

> **Status:** Project concept and prototype specification. Benchmark
> values in the presentation are illustrative placeholders, not measured
> results. Replace them with actual results before making performance
> claims.

## Problem

A multi-sensor system may receive reports with different noise levels,
reporting frequencies, clock offsets, missing packets, and uncertain
sensor positions. Naive fusion can create duplicate tracks, unstable
identities, misleading confidence, or stale estimates that look current.

## Proposed solution

AIRWEAVE combines: - **Sensor simulation:** independently configurable
synthetic nodes and ground truth. - **Time alignment and quality
checks:** timestamps, report age, missing data, and node health. -
**State estimation:** position and velocity estimates with
uncertainty. - **Association awareness:** gated matching with explicit
ambiguity reporting. - **Resilience testing:** clock offsets, packet
loss, sensor disconnection, and recovery. - **Evaluation:** repeatable
comparison against independent synthetic ground truth. -
**Evidence-first UI:** show track status, contributing observations,
uncertainty, and event history.

## Intended users and impact

- **Systems engineers** validating sensor-fusion designs under
  reproducible failure conditions.
- **Research teams** comparing estimator and association approaches.
- **System integrators** evaluating the impact of sensor quality and
  communication degradation.
- **Safety and assurance teams** reviewing provenance, confidence, and
  failure behavior.

Potential impact is better engineering visibility into degraded data,
more reproducible testing, and fewer unsupported assumptions about what
the system knows. AIRWEAVE is not a certified safety system and does not
replace human judgment or field validation.

## Architecture

```text
Synthetic ground truth
        |
        v
Independent sensor-node simulators
(noise, rate, clock offset, packet loss)
        |
        v
Input validation and timestamp alignment
        |
        v
Observation association + state estimation
        |
        v
Track state, uncertainty, health and provenance
        |
        +--------------------+
        |                    |
        v                    v
Live dashboard         Benchmark evaluator
                       (ground-truth comparison)
```

### Suggested implementation stack

- Python 3.11+
- NumPy and SciPy for simulation and numerical operations
- FastAPI and WebSockets for a live backend, if needed
- Streamlit for the fastest functional dashboard, or React +
  TypeScript for a more customized UI
- pytest for automated tests
- pandas for benchmark summaries

The first version should be a centralized, deterministic reference
implementation. Distributed execution and message brokers can be added
after the core estimator and tests are reliable.

## Demo scenarios

1.  **Normal operation:** three simulated nodes report observations.
2.  **Clock disagreement:** one node has an offset and messages arrive
    asynchronously.
3.  **Network degradation:** packet loss rises and one sensor
    disconnects.
4.  **Ambiguous association:** two synthetic objects pass close
    together.
5.  **Recovery:** a sensor reconnects and the system handles returning
    observations without silently rewriting history.

## Metrics

Report real measured results for: - Position RMSE and velocity RMSE
against independent ground truth. - Track continuity and track
fragmentation. - Identity switches. - Track update age and percentage of
time with an available estimate. - Uncertainty calibration, when
sufficient samples are available. - Update latency, CPU usage, and
memory use. - Performance as packet loss and sensor outages increase.

Always include scenario settings, sample counts, random seed, and
baseline definition. Do not hardcode attractive scores or claim that
fusion always outperforms a single sensor.

## Development plan

### Milestone 1 --- Reproducible simulator

Create deterministic ground truth and three independently configured
sensor streams. Add unit tests for generated observations and fault
injection.

### Milestone 2 --- Baseline estimator

Implement a documented Kalman filter for position and velocity. Define
coordinate frames, units, process noise, and measurement noise.

### Milestone 3 --- Association and time handling

Add timestamp normalization, stale-data handling, gating, association
ambiguity, and track lifecycle rules.

### Milestone 4 --- Dashboard

Visualize sensor health, observations, estimates, uncertainty, track
age, and the event timeline.

### Milestone 5 --- Evaluation

Run repeatable tests against ground truth, compare with a single-sensor
baseline, and generate benchmark reports.

### Milestone 6 --- Hardening

Add tests for malformed messages, missing data, sensor outage/recovery,
deterministic replay, and performance profiling.

## Getting started

This repository is a project specification. Once implementation files
are added, a suggested local workflow is:

```bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
source .venv/bin/activate
```

Then install the dependencies defined by the implementation's
`requirements.txt` and follow the actual startup command documented by
the application. Do not treat this conceptual README as evidence that an
executable application already exists.

## Limitations and responsible use

- All initial observations are synthetic and may not represent
  real-world sensor physics.
- Results depend on the simulator, assumptions, noise model, and test
  distribution.
- A Kalman filter can be inappropriate for strongly nonlinear or
  multimodal motion; document the chosen model and its failure modes.
- Confidence scores and covariance estimates require calibration and
  validation.
- Simulated resilience is not proof of field reliability.
- The prototype is limited to situational awareness, tracking
  research, and test evaluation. It does not implement weapon control,
  autonomous interception, engagement recommendations, or autonomous
  targeting.

## Presentation

The accompanying `AIRWEAVE_Project_Presentation.pptx` contains six
slides: title, problem, solution/features, impact, demo, and questions.
The demo slide labels its sample metrics as illustrative placeholders.
Replace these with measured values from the completed implementation.
