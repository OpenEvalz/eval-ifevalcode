# eval-ifevalcode

**IFEvalCode: Controlled Code Generation**

**Paper:** https://arxiv.org/abs/2507.22462

Evaluates code generation models on their ability to produce correct code while adhering to
specific instruction constraints across 8 programming languages.

## At a glance

| | |
|---|---|
| Upstream | [`src/inspect_evals/ifevalcode`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/ifevalcode) |
| Group | Coding |
| Total samples | 810 |
| Execution class | `sandbox-local` |
| Cost class | `high` |
| Flags | sandboxed |
| Tags | — |

### Tasks

| Task | Samples |
|---|---|
| `ifevalcode` | 810 |

### External assets

- `huggingface` — `Multilingual-Multimodal-NLP/IfEvalCode-testset` (pinned)

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/ifevalcode \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
