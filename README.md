# infinigram

Two notes from the HalluTrace senior project, on the corpus search tool the whole method depends on.

**Read them here: https://erthcyp.github.io/infinigram/**

| Page | What it covers |
|---|---|
| [`index.html`](index.html) | How I investigated infini-gram. A token boundary problem that inflated one count by a factor of 9.4, why the obvious fix fails under byte pair encoding, and the procedure that gives exact counts. |
| [`value.html`](value.html) | What we add over HALoGEN. Which tool it used, which corpora it actually searched, and a first measurement of what searching a substitute corpus costs. |

Every figure was measured through the public [infini-gram](https://infini-gram.io/) API in September 2026,
against Dolma v1.7 (2.6T tokens) and C4-train (200B tokens). The runnable code is at the end of `index.html`.

## Headline findings

- A naive n-gram count of `np.float` was inflated by **89.4 percent**, because the tokenizer makes the correct form
  `np.float64` a strict extension of it. Correcting this flipped the provenance verdict from one end of our framework
  to the other.
- Anchoring a query with a closing character loses **39 percent** of legitimate matches, because byte pair encoding
  merges the anchor with whatever follows it.
- The next-token distribution endpoint returns approximate counts, off by up to **6.5 percent**, with a visible
  power-of-two quantization signature. Use it to identify tokens, never to count them.
- Searching C4 in place of a model's own corpus changed the label in **one case out of four** on a preliminary sample.

## Reproducing

Standard library Python only. No installation, no API key. The script at the end of `index.html` runs in about a
minute.

## References

- infini-gram, [arXiv:2401.17377](https://arxiv.org/abs/2401.17377)
- HALoGEN, [arXiv:2501.08292](https://arxiv.org/abs/2501.08292)
- WIMBD, [arXiv:2310.20707](https://arxiv.org/abs/2310.20707)
