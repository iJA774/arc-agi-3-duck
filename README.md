# ARC-AGI-3 Duck: Persistent Python

A Kaggle notebook extending the Duck harness with a persistent Python interpreter for each game level, an offline CPython 3.12 runtime, and lightweight execution diagnostics.

The implementation builds on [Tufa Labs' Duck harness](https://github.com/Tufalabs/duck-harness), [Daniel Franzen's Milestone 2 solution](https://github.com/da-fr/arc-agi-3-solution), and [Helios Li's persistent-Python notebook](https://www.kaggle.com/code/heliosli/try-merging-it-you-might-get-lucky-and-win-gold).

## Notebook

[arc-agi-3-duck-v7-full-run.ipynb](arc-agi-3-duck-v7-full-run.ipynb)

The original filename and notebook commentary are retained. This README describes the current executable configuration; some historical notebook text describes earlier public-run settings.

## How it works

The agent observes a game, writes Python, executes actions, and reasons from the resulting observations. Variables, helper functions, queues, and plans remain available between tool calls within a level. Level changes, game changes, worker failures, and timeouts clear ordinary Python state. The existing function-retention mechanism remains available for eligible helpers.

Saved action references dispatch through the current call's guards and bookkeeping. Retained animation views refresh between calls and actions. Worker cleanup coordinates stream ownership, and memory-error handling attempts to preserve live variables for subsequent cleanup. The worker keeps a 1 GiB address-space ceiling, with 896 MiB available during user-code execution and the remainder reserved for communication and recovery.

The notebook retains the inherited Qwen3.8-Flash-Next W4A16 AutoRound model, Albucino draft checkpoint, Pennyroyal SGLang v2.5.3 serving stack, ten active inference requests, and three speculative decoding steps.

## Run on Kaggle

1. Import the notebook into Kaggle.
2. Select **RTX PRO 6000** and keep **Internet disabled**.
3. Attach all five inputs below.
4. Run all cells. The default non-submission path only writes the preparation parquet.

| Input | Kaggle resource |
| --- | --- |
| Competition | `arc-prize-2026-arc-agi-3` |
| Source bundle | `dfranzen/taaf-kaggle-source-bundle-copy` |
| Offline wheels | `dfranzen/pennyroyal-v253` |
| Main model | `dfranzen/intel-qwen3.8-flash-next-w4a16-autoround/transformers/default/1` |
| Draft model | `dfranzen/albucino-qwen3-8-flash-next-drafter/transformers/default/1` |

Weights, game environments, and offline wheels are attached separately and are not included in this repository. The installer resolves dependencies from local inputs and does not fetch packages or source from GitHub. Solver and serving processes use CPython 3.12; the notebook kernel may use a different version.

## Execution modes

```python
PLACEHOLDER_NON_SUBMISSION = True
PYTHON_STATE_MODE = "level"
```

| Mode | Behavior |
| --- | --- |
| Normal Save & Run, default settings | Write a placeholder `submission.parquet`; skip solver setup, model loading, and gameplay. |
| Normal run with `PLACEHOLDER_NON_SUBMISSION = False` | Evaluate all available public games once. |
| Competition rerun | Execute the full solver against the competition gateway, regardless of the placeholder switch. |

Competition mode is selected when Kaggle's `KAGGLE_IS_COMPETITION_RERUN` variable is `1` or `true` (case-insensitive). Do not set it manually for public evaluation. The placeholder row is a packaging artifact, not a measured score.

Full runs share a **nine-hour wall-clock budget**, including setup and model loading, with **180 seconds reserved for shutdown**. To use function retention without ordinary per-level state, set `PYTHON_STATE_MODE = "functions"`. Custom input roots can be supplied through `INPUT_OVERRIDES`.

## Outputs

Full solver runs produce lightweight records including `run_manifest.json`, `run_summary.json`, and `serve.log`. Heavy HTML, video, transcript, and model-request exports are disabled. Placeholder-only runs do not produce solver diagnostics.

The notebook embeds the harness patch and persistent-worker assets so it can be imported as a single file. No training step or model-weight update is performed.

## Acknowledgements and licensing

- **Jeroen Cottaar and Tufa Labs:** Duck harness and game-agent infrastructure.
- **Daniel Franzen:** Milestone 2 solver changes, model configuration, and serving setup.
- **Helios Li:** per-level Python persistence implementation.
- **Pennyroyal contributors:** [SGLang RTX PRO 6000 runtime](https://github.com/jpezzulli/sglang-rtxpro6000).

See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for source lineage and license scope. Original contributions in this repository are provided under Apache-2.0; third-party material retains its applicable upstream terms. Model weights and externally attached dependencies are licensed separately.
