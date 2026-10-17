# BSL Scout Agent

Local code research for 1C:Enterprise / BSL, inspired by ColGrep-Agent.

**Status: research and design.** This repository currently contains the project proposal and evaluation roadmap. There is no working indexer, MCP server, trained encoder, or agent release yet.

## Goal

A larger coding model delegates repository exploration to a small local specialist. The specialist searches a configuration export, inspects code and metadata, follows relevant relationships, and returns compact evidence with source locations and unresolved questions.

The specialist should generalize across configurations. ERP is a possible training environment; the user's custom product and other configurations are transfer-evaluation targets, not knowledge to hard-code into the model.

## Proposed direction

- Parse Configurator XML/BSL and EDT exports into a shared representation.
- Combine exact lexical search with ColBERT-style multi-vector retrieval.
- Evaluate the unmodified `lightonai/LateOn-Code-edge` before adapting it to Russian queries and BSL.
- Expose one research engine through a small MCP interface; consider a CLI using the same engine.
- Train retrieval and agent behavior separately, only after establishing baselines.
- Return verified source locations, configuration identity, index snapshot, and coverage limits.

The public repository will contain original code, documentation, synthetic examples, and publishable evaluation assets. Configuration dumps, proprietary source code, local indexes, credentials, and model weights belong outside version control.

## Documents

- [Design proposal, in Russian](docs/design-proposal.md)
- [Research notes and primary sources](docs/research.md)
- [Evaluation roadmap](docs/roadmap.md)
- [License policy and examples](docs/licensing.md)
- [Repository working instructions](AGENTS.md)

## Inspiration

[ColGrep / NextPlaid](https://github.com/lightonai/next-plaid), [ColGrep-Agent-2B](https://huggingface.co/lightonai/colgrep-agent-2B), and [PyLate](https://github.com/lightonai/pylate) provide references for local retrieval, late interaction, and specialized search agents. The 1C community provides important examples of BSL indexing, metadata navigation, and token-efficient tools; see the research notes.

This is an independent project, not an official LightOn or 1C product. No third-party implementation or model weights are bundled in this initial repository.

## License

Original repository content is offered under the [PolyForm Perimeter License 1.0.1](LICENSE) from this revision forward. This is source-available software.

Internal use, including use for commercial development, is permitted. Providing others with a competing product or service based on this software requires separate permission; the restriction can also apply to free competing offerings. Redistribution must preserve the license and [required notices](NOTICE). See [the policy and examples](docs/licensing.md); the license text governs.

Earlier revisions published under MIT retain their original license permissions. External code, models, and datasets retain their own licenses and publication conditions.
