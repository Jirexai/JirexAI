# Filesystem — research

**Status:** In Design

A filesystem built under the Logos method — Logos-structured storage blocks, multi-dimensional capability permissions, rhythmic snapshots, and native compression via the research-program codec.

## The thesis

Every filesystem in production today was designed in the 1990s or earlier. `ext4`, `btrfs`, `zfs`, `xfs`, `apfs` — all assume bytes as the native storage unit, UTF-8 as the filename encoding, POSIX `rwx` as the permission model, and link-count as the ownership abstraction. Each of these is a compromise inherited from a different era of operating-system design.

A filesystem under the Logos method starts from a different premise: storage is soil, files are seeds, permissions are not bits but capabilities, and the rhythm of work-and-rest is the governance model.

## What's specified

### Storage blocks

Native allocation unit is a Logos-structured block sized to align with modern SSD and rotational-disk sectors. Sector-alignment is a property, not a fight.

### Multi-dimensional permissions

POSIX `rwx × 3 roles = 9 bits` is replaced by a capability model along multiple orthogonal dimensions, each a graded level rather than a single bit. Finer access control than the POSIX triple without the complexity of a full ACL system.

### Rhythmic snapshots

Every file accumulates a history. Periodically — on a rhythmic cadence tied to the operating system's work/rest cycle — the history is consolidated into a durable snapshot. After a further period, the snapshots themselves are consolidated into a jubilee checkpoint. The filesystem has long-term memory built into its rhythm, not as an optional `zfs snapshot` command.

### Native compression via germination

Files are stored in the research codec's germinated form (see [compression-codec.md](./compression-codec.md)). Applications reading files see uncompressed bytes; the filesystem handles the transparent "germination" on read. Applications that are themselves codec-native read the seed form directly, with zero translation overhead.

### No orphan links

POSIX hard links break the "one file, one location" property and introduce link-count bookkeeping that the filesystem has to maintain forever. The Logos filesystem registers every file under an explicit covenant in the identity service — "links" are signed covenants between two filesystem objects, not a cryptic `nlink` counter.

### Replication by witness

The critical-data replication model is `n`-way, with a distinguished witness node that takes read-only copies and runs integrity-verification passes against them. "Two or three witnesses" as a filesystem discipline.

## Deployment path

1. **FUSE userspace.** Linux / macOS / BSD, mounted via `libfuse`. Any application sees it as a normal POSIX filesystem, with the underlying model running in userspace. This is the first usable form.
2. **Linux kernel module.** Same behavior, native kernel performance.
3. **Native on the research operating system.** The filesystem and OS are the same substrate. No translation. No POSIX compatibility layer except as a gateway for legacy applications.

## Why now

The filesystem depends on the research compression codec and the research capability-permission model; both are early-phase research themselves. FUSE implementation is additive — it can begin once the codec has a working prototype.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For research collaboration, formal-verification partnership, or domain applications (medical records, compliance-heavy environments, archival), reach out via [jirex.ai](https://jirex.ai).
