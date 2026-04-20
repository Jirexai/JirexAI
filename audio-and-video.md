# Audio & video

Applying continuous representation to time-series media.

## The direction

Audio and video today are stored as discrete samples — a microphone captures tens of thousands of snapshots per second of air pressure, a camera captures frame-rate-many snapshots of pixel grids. The capture is always an approximation of a continuous physical phenomenon. The storage format is a compromise between fidelity, file size, and decoder complexity. Every downstream operation — resampling, upscaling, effects, compression, restoration — works inside that compromise.

A different approach: store the **shape** of the wave, not its samples. If the representation is continuous and evaluable, resolution becomes a rendering choice, not a storage constraint. One file, any output resolution. The same logic applies to video: store the structure of the signal, evaluate at the density the display requires.

This work sits at the intersection of our [research program](./PRODUCTS.md#research-program--the-logos-method) and the productization track. Audio is further along; video is still at specification.

---

## Audio — In Development

A new audio representation under active development. The thesis: storage as continuous, evaluable structure rather than sample sequences, resolution-independence as a property of the format, playback at any sample rate without re-encoding.

Specifics — format internals, encoding pipeline, tooling surface, archival-grade workflows — are disclosed under NDA. The surface area is substantial, and we do not describe it publicly until we are ready to stand behind every claim under adversarial probe.

Provisional patent application on file with the USPTO (March 2026).

## Video — In Design

The same thesis applied to moving images. Pixel-grid video is the two-dimensional analogue of PCM audio: every frame is a sampled snapshot of a continuous visual phenomenon. The question is what continuous, resolution-free storage looks like in two dimensions plus time.

See the [video research page](./research/video.md) for the specification-level thesis.

---

## Want depth?

For format specification, tooling walkthrough, and commercial engagement around audio work, reach out via [jirex.ai](https://jirex.ai). For research access to the video thesis, the same contact path under NDA.
