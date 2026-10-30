# Attention Is All You Need (2017)

> Replaces recurrence and convolution in sequence transduction with stacked
> self-attention, introducing the Transformer.

| | |
|---|---|
| **Paper** | [arXiv:1706.03762](https://arxiv.org/abs/1706.03762) |
| **Authors / venue** | Vaswani, Shazeer, Parmar, Uszkoreit, Jones, Gomez, Kaiser, Polosukhin — NeurIPS 2017 |
| **Code** | [bartuonder/attention-is-all-you-need](https://github.com/bartuonder/attention-is-all-you-need) |
| **Framework** | PyTorch |
| **Status** | 🧪 Implemented & tested |

## What was implemented

Written to match the paper rather than `nn.Transformer` or later variants (no
Pre-LN, no GELU, no RoPE, no extra bias on attention projections).

| Paper | Code |
|---|---|
| Eq. 1 scaled dot-product attention | `transformer/attention.py` |
| §3.2.2 multi-head attention | `MultiHeadAttention` |
| Eq. 2 position-wise FFN (ReLU) | `transformer/layers.py` |
| §3.1 Post-LN residual `LayerNorm(x + Sublayer(x))` | `ResidualLayerNorm` |
| §3.4 embeddings × √d_model, tied pre-softmax weights | `TokenEmbedding`, `Transformer.generator` |
| §3.5 sinusoidal positional encoding | `SinusoidalPositionalEncoding` |
| §3.1 / §3.2.3 causal and padding masks | `transformer/masking.py` |
| Eq. 3 Adam warmup schedule | `transformer/schedule.py` |
| §5.4 residual dropout, label smoothing ε_ls = 0.1 | `config.dropout`, `LabelSmoothingLoss` |
| §6.1 beam search, beam 4, length penalty α = 0.6 | `transformer/decode.py` |

## Verification

Parameter counts with a 37k shared vocabulary (Table 3):

| Model | Paper | Ours |
|---|---|---|
| base | 65M | 63.0M (63,045,632) |
| big | 213M | 214.2M (214,171,648) |

`tests/test_paper.py` also checks: the sinusoidal PE formula, warmup/decay
meeting at `warmup_steps`, causal mask blocking the future, attention equal to
softmax of scaled scores, near-zero attention on padding keys, embedding
scaling and weight tying, no future leakage in the decoder, a bidirectional
encoder, label-smoothing probability mass, GNMT length penalty, and a tiny
model overfitting the reverse task.

Base hyperparameters (Table 3): `N=6 d_model=512 d_ff=2048 h=8 d_k=d_v=64
P_drop=0.1 ε_ls=0.1`, Adam `β1=0.9 β2=0.98 ε=1e-9`, `warmup=4000`.

## Faithfulness and scope

- **Implemented as specified:** Post-LN, sinusoidal PE, shared embedding /
  pre-softmax weights, scaled dot-product and multi-head attention, ReLU FFN,
  residual dropout, label smoothing, inverse-sqrt LR schedule, greedy and GNMT
  beam search.
- **Not reproduced:** WMT 2014 En-De / En-Fr BLEU (Table 2), BPE vocabulary,
  8×P100 training and checkpoint averaging. These are experimental protocol
  rather than architecture, and no BLEU numbers are claimed.
- **Training entry point:** a reverse-sequence toy task (`train.py`), used to
  confirm the model learns an encoder–decoder alignment on a laptop.

```bash
python -m pytest tests/ -q
python train.py --config tiny
```
