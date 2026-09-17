# RoboFlywheel-Sim: A Cross-Engine Simulation Platform for Robot Data Generation and Policy Evaluation

**From scenes to robot learning data and reproducible evaluation.**

> **Pre-release preview.** RoboFlywheel-Sim is under active development. The source code is not yet publicly available, and an open-source release is planned. Release details will be announced in this repository.

![Cross-engine scene preview with Unitree G1 in Isaac Sim, MuJoCo, and Genesis](assets/cross-engine-simulation.png)

*A Unitree G1 scene shown in Isaac Sim, MuJoCo, and Genesis. Rendering appearance varies across engines.*

## Overview

Scaling robot learning requires diverse interaction data and reproducible evaluation across environments. However, simulation workflows remain fragmented across physics engines, asset formats, and task interfaces, making it difficult to reuse resources and compare policy behavior.

RoboFlywheel-Sim brings asset reuse, simulation execution, data collection, and policy evaluation into a shared workflow across **MuJoCo, Isaac Sim, and Genesis**. A common scene representation connects assets to different engines, while unified task and recording interfaces organize actions, observations, trajectories, and evaluation results.

Our broader goal is to bring generated scenes and artist-authored assets into the same pipeline, enabling researchers to move from a scene and a task to robot learning data and reproducible evaluation across robot embodiments.

## Platform Highlights

- **Cross-engine workflows.** Run supported scenes and tasks through a common interface across MuJoCo, Isaac Sim, and Genesis.
- **Reusable scene assets.** Represent scenes, robots, physical properties, and visual resources in a structured USD-based format, with a separate path for native simulation assets.
- **Transparent asset conversion.** Make approximations and unsupported features explicit when adapting assets to a target engine.
- **Robot data collection.** Record actions, states, and camera observations; select successful trajectories and export data for robot learning workflows, including LeRobot.
- **Unified policy evaluation.** Connect policies to shared task definitions, outcome metrics, trajectory replay, and versioned experiment configurations.

## Framework Overview

![RoboFlywheel-Sim framework overview showing compiled and native execution paths, shared orchestration, and isolated engine workers](assets/architecture.png)

*RoboFlywheel-Sim connects scene and task authoring, simulation and control, and data and evaluation. Compiled scenes and native programs share experiment orchestration and an Engine Abstraction Layer (EAL), with each simulation backend hosted in an isolated worker process. Ray optionally schedules complete evaluation trials. Long-horizon tasks and sim-to-real transfer are planned research workflows.*

SceneIR is RoboFlywheel-Sim's scene intermediate representation. The compiler produces engine-specific build plans and reports conversion approximations and unsupported features. Native execution delegates loading to the source engine or an application provider. Task scoring supports rule-based evaluation and vision-language model (VLM) evaluation.

## Usage Preview

Our goal is to make a scene, a robot, and a task the starting point for both data collection and policy evaluation.

> **Proposed CLI — not yet available.** These conceptual examples illustrate the intended user experience. Commands, parameters, and supported workflows may change before release. Policy configurations and asset paths are illustrative.

### Collect data from a generated scene

Describe a scene and task, choose a robot and engine, and supply a policy to generate demonstrations.

```bash
# Conceptual example: natural-language scene generation is planned.
roboflywheel-sim collect \
    --scene "a kitchen with a cup and a plate on the table" \
    --robot franka_panda \
    --task "pick up the cup and place it on the plate" \
    --policy configs/demonstration_policy.yaml \
    --engine mujoco
```

The intended workflow records actions, robot states, and camera observations for trajectory review and export to robot learning datasets.

### Evaluate a policy across simulation engines

Evaluate a policy on a task suite across multiple engines with a single CLI command. The `--engine` option accepts a comma-separated list of engine names.

```bash
# Proposed CLI: the suite defines scenes, robots, tasks, and metrics.
roboflywheel-sim eval benchmarks/manip_suite/suite.yaml \
    --policy configs/my_policy.yaml \
    --engine mujoco,isaacsim,genesis
```

The platform runs the suite on each selected engine and records task success metrics, trajectories, and experiment configurations for comparison. Execution depends on each engine's supported capabilities; conversion reports make approximations and unsupported features visible.

## Roadmap

The planned initial release focuses on the simulation framework, supported engine integrations, and reproducible examples for data collection and evaluation.

- [ ] Publish the core framework and engine adapters.
- [ ] Release installation guides and runnable examples.
- [ ] Document supported features and engine-specific limitations.

Further development targets natural-language scene construction, broader robot embodiment support, and expanded data generation workflows. Planned validation studies include long-horizon task evaluation and sim-to-real policy transfer.

## Release Status

This repository currently serves as the public project preview. Source code, installation instructions, and runnable examples will accompany the public release. The release date and licensing details have not yet been announced.

Watch this repository for release announcements. Feedback and research use cases are welcome through GitHub Issues.
