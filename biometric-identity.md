# Biometric identity

Continuous-representation biometric identification for consumer-grade hardware, without machine learning on the matching path.

## The direction

Every mainstream biometric system — face, fingerprint, voice, gait — builds its identity model through neural networks trained on large corpora. This works, but it inherits the properties of deep learning: opaque decision boundaries, vulnerability to adversarial inputs, cloud-scale compute at inference, and a template that lives inside weights nobody can audit.

A different approach: represent the physiological signal as a small number of parameters that capture the identity-bearing shape of the waveform — and match by direct distance in parameter space. No neural network. No inference tax. No cloud dependency. The template is a handful of coefficients auditable by inspection.

This track sits at the intersection of our [identity & transport layer](./identity-and-transport.md) (cryptographic identity for machines) and the consumer side of the stack: cryptographic identity is for servers, biometric identity is for the humans those servers serve.

---

## Cardiac identity — In Development

Identification from heartbeat waveforms captured by consumer-grade physiological sensors. The signal is segmented into beat units, aligned, and fit to a small set of parameters. Matching is direct distance in a low-dimensional space.

Validated on public cardiac datasets with accuracy comparable to the best ML-based approaches in the literature. Numbers, dataset names, and reproducibility steps disclosed under NDA.

## Vocal identity — In Development

Speaker identification from the spectral envelope of voice audio. Same discipline as the cardiac work: the envelope is fit to a small number of parameters; matching is direct. No MFCC, no deep embeddings, no acoustic model.

Validated on a widely-used public speaker corpus. Numbers and methodology disclosed under NDA.

## Bimodal fusion — In Development

Cardiac and vocal identity combined under a single confidence gate. The two modalities have complementary failure modes — the cardiac channel is hard to forge but requires a sensor; the vocal channel is universally accessible but easier to imitate. Together they close the gap each leaves open.

The bimodal implementation targets phone-class hardware plus a chest-strap cardiac sensor — no server-side inference, no cloud dependency, no ML at runtime. Enrolment takes under a minute; identification takes a few seconds.

---

## Privacy

Templates are parameter coefficients, not raw biometric data. They cannot be reverse-engineered to the original signal. Raw enrolment data is discarded after template derivation. Matching is self-contained — the device running the check does not need to send anything to a server.

## Want depth?

For measured accuracy on named public datasets, hardware integration guidance, mobile SDK access, patent disclosures, or commercial engagement, reach out via [jirex.ai](https://jirex.ai).
