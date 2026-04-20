# Compression codec — research

**Status:** In Design

A compression codec built on a universal pre-Babel text substrate and structural decomposition rather than on dictionary-based statistical redundancy removal.

## The thesis

DEFLATE, LZMA, Brotli, Zstandard — every mainstream general-purpose compressor attacks *one* dimension of redundancy: exact byte repetition. They do this well. What they do not touch at all is:

- **Semantic redundancy.** Three different Unicode normalizations of the same word compress as three different strings.
- **Structural redundancy.** A gradient, a wave, a repeating pattern — all compress as raw samples, because DEFLATE cannot see the shape underneath.
- **Writing-system bias.** UTF-8 is a historical artifact that costs Chinese text three bytes per character while giving English one. The compression stage inherits the bias.

A codec under the Logos method attacks all three at once.

## How it works

### Stage one — universal text encoding

Input passes through a text layer that maps every written script in the world to a shared internal representation. The output of stage one is writing-system-neutral: Chinese, Arabic, Hebrew, and English occupy comparable space per semantic unit. Normalizations are collapsed; invisible-character tricks are neutralized.

### Stage two — structural decomposition

The normalized stream is interpreted as a trajectory, and structural shape is extracted as a small set of primitive descriptors. A long smooth gradient becomes a handful of parameters. A repeating pattern becomes a primitive identifier plus its arguments. Irregular data falls through to primitive-sample encoding with no penalty beyond the substrate change.

The primitive vocabulary is fixed — the codec does not transmit a dictionary. Reader and writer share the same set of fundamental shapes by definition.

## The metaphor

The file format does not "decompress" — it **germinates**. The file is a seed: the minimal representation that carries the code to reconstruct the original. Given the seed and an evaluator, the original reconstitutes — as a seed given soil and water reconstitutes a tree.

The API reflects this: `from_file()` plants, `germinate()` reconstructs, `save()` stores. Nothing in the API speaks of "extraction" or "decompression."

## Expected behavior

- **On structured continuous data** (audio waveforms, scientific measurements, sensor traces, gradients, vector graphics) — substantially better than DEFLATE.
- **On multilingual text** — better than UTF-8-bound compressors, because the inputs normalize first.
- **On irregular data** — comparable; dominated by the substrate efficiency rather than the structural layer.

## Why now

The codec depends on the universal text layer being production-ready for all scripts, which is a live extension track in the core platform. It also benefits from the language and operating-system research being close enough that the structural layer can be the native internal representation.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For collaboration on the text layer, codec performance benchmarking, or specialized domain applications (medical imaging, scientific instrumentation, biological data), reach out via [jirex.ai](https://jirex.ai).
