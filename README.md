# Paper to Code

An index of research papers I have re-implemented from scratch. Each paper lives
in its own repository; this repo is the map: what was implemented, how it was
verified, and which numbers were actually reproduced.

**Principles**

- **From scratch.** No high-level wrappers such as `nn.Transformer` or `torchvision.models`; the code follows the paper's equations and section numbers.
- **Verified against the paper.** Tests assert quantities the paper states (parameter counts, FLOPs, masking behaviour, schedules).
- **Honest about scope.** Every entry says what was reproduced, what was only implemented, and what was not run.

## Status legend

| Badge | Meaning |
|---|---|
| ✅ Reproduced | Training run completed and results compared with the paper's numbers |
| 🧪 Implemented & tested | Architecture implemented and checked by tests; paper-scale training not run |
| 🚧 In progress | Work in progress |

## Index

| # | Paper | Year | Area | Code | Status | Headline result |
|---|---|---|---|---|---|---|
| 1 | [Attention Is All You Need](https://arxiv.org/abs/1706.03762) | 2017 | NLP / sequence modelling | [code](https://github.com/bartuonder/attention-is-all-you-need) · [notes](papers/attention-is-all-you-need.md) | 🧪 Implemented & tested | base 63.0M params (paper 65M); WMT BLEU not run |

Per-paper write-ups (paper → code map, results, deviations) are in [`papers/`](papers/).
New entries start from [`papers/_template.md`](papers/_template.md).

## License

[MIT](LICENSE)
