# Third-party notices

This notebook is a modified integration of existing ARC-AGI-3 work. It does not claim authorship of the underlying models, Duck harness, Franzen baseline, or Helios Li persistence implementation.

## Source lineage

- Tufa Labs / Jeroen Cottaar: https://github.com/Tufalabs/duck-harness . The notebook identifies upstream base commit `7652836`; its bundled patch is applied to the separately mounted source bundle.
- Daniel Franzen: https://github.com/da-fr/arc-agi-3-solution and https://www.kaggle.com/code/dfranzen/arc-agi-3-milestone-2-solution . The source repository distributes an Apache-2.0 license; a copy is included as `LICENSE`.
- Helios Li: https://www.kaggle.com/code/heliosli/try-merging-it-you-might-get-lucky-and-win-gold . The notebook retains attribution to its per-level interpreter implementation.
- Pennyroyal serving runtime: https://github.com/jpezzulli/sglang-rtxpro6000 . Runtime wheels are externally attached, not distributed here.

## Modifications

The notebook contains changes for current-call action dispatch, animation-view refresh, offline CPython 3.12 dependency setup, input discovery, shared runtime budgeting, reduced diagnostic exports, worker memory recovery, stream cleanup, and non-submission placeholder packaging. Existing notebook attribution is preserved.

Apache-2.0 applies to this repository's original contributions and to upstream portions distributed under that license. This notice does not relicense third-party material. Retain applicable upstream copyright, license, and attribution notices when redistributing derived work. Check the respective source, dataset, and model licenses for externally attached resources.
