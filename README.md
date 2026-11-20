# Terminal-Bench-Science Harbor tasks

Task data, environments and verifiers for the following PenguinHarness packages.

- [sec-d](penguinharness-benchmark-sec-d/README.md) · [package](https://github.com/Prism-Shadow/penguin-harness-benchmark/tree/main/packages/penguinharness-benchmark-sec-d)

## Running a task (for agents)

Check out this data repository at the full commit in the Case statement. From its
root, run the statement's Harbor command with the [shared Penguin adapter](https://github.com/Prism-Shadow/penguin-harness-benchmark/tree/8a507af0329fed8f9a1baf44d1364b438340c78a/agents).
To prepare the adapter once for all datasets, use an empty directory:

```bash
git clone --filter=blob:none --sparse --no-checkout https://github.com/Prism-Shadow/penguin-harness-benchmark.git ../penguin-benchmark-adapter
git -C ../penguin-benchmark-adapter sparse-checkout set agents
git -C ../penguin-benchmark-adapter checkout --detach 8a507af0329fed8f9a1baf44d1364b438340c78a
export ADAPTER_DIR="$(git -C ../penguin-benchmark-adapter rev-parse --show-toplevel)/agents"
export PYTHONPATH="$ADAPTER_DIR"
```

Reuse that checkout for other datasets pinned to the same adapter commit, setting
`ADAPTER_DIR` to its absolute `agents/` directory. Run the package's `job.yaml`
from the data repository root for a batch, or use the Case command for one trial.
Use the model, time limits and network settings in the Case. The default host
limit is four concurrent trials. For Cases that use `shared-network.yaml`,
[create the `penguin-bench` network once per Docker host](https://github.com/Prism-Shadow/penguin-harness-benchmark/blob/8a507af0329fed8f9a1baf44d1364b438340c78a/agents/shared-network.yaml#L6) before the first trial.
For a network-restricted agent, `"$ADAPTER_DIR/agent_host.py"`
resolves the model's host from the configured Project. The adapter options are
in its [README](https://github.com/Prism-Shadow/penguin-harness-benchmark/blob/8a507af0329fed8f9a1baf44d1364b438340c78a/agents/README.md).

Harbor writes the trial's `result.json` under the job output directory. Score
the verifier's reward as the Case rubric specifies. If the verifier produced no
reward or the model never ran, record an evaluation failure. Retry startup
failures after fixing their cause, under a new job name.
