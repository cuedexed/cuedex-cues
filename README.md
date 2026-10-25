# CueDex shared cues

Cue sets for CueDex shows — when a track plays, which lighting / video / audio
cue fires at which point of the music — shared by the people who wrote them. Everything here is signed by its author's
key and checked by a robot on every pull request. CueDex reads this repository directly (it is CueDex's default shared
source) or as an offline bundle for a venue without a network.

## Licence: CC0 1.0

Every cue set in this repository is released under **CC0 1.0 Universal** (public domain dedication, [LICENSE](LICENSE)).
Anyone may use, change and share it for any purpose, without asking and without credit. Credit is welcome and CueDex shows
each author's chosen name and key, but it is not a condition.

## Contributor agreement (`cuedex-contributor-1`)

By publishing a cue set (CueDex asks once, and every signed set carries the agreement's id) you confirm that:

1. the cue set is your own work, or you have the right to release it;
2. you release it under CC0 1.0;
3. it contains **no audio, no lyrics and no other third-party content** — only cue positions, lane names, typed cue actions
   for the listed target profiles, and the recording's title / artist / duration (plus optional ISRC / MusicBrainz id /
   platform ids you supply);
4. the copies already shared under CC0 cannot be recalled: you can remove your set from this repository at any time, but
   copies made before stay valid.

## What is stored here

- `sets/<set>/<version>.json` — one **signed version** of a cue set, exactly as its author's CueDex signed it: the
  recording's title, artist and duration (optional ISRC, MusicBrainz id, platform ids), the author's optional display
  name, the set's title and notes, licence + agreement id, the author's clock time, and the cues (positions in samples
  at 48 kHz on the recording's timeline, lanes with a target profile id, typed actions). The author's **public key** (an
  ed25519 key made on their Mac — a pseudonym, not an account) and signature. File name = the content hash.
- `fingerprints/<hash>.fp` — the recording's **fingerprint**: landmark hashes and a coarse 100 Hz onset envelope,
  numbers that identify a recording and align a copy to it. Never audio; they cannot be turned back into sound.
- Derived by the check, never edited by hand: `index.json`, `recordings/<id>.json`, `sets/<set>/set.json`.

Not stored: e-mail addresses, names other than a display name an author chooses to publish, accounts, audio, lyrics, IP
addresses. GitHub itself keeps the account that opens a pull request, as on any repository.

## How a set gets here

1. In CueDex: **Library → Shared**, Source `repo:/path/to/your/clone/of/this/repository`, **Publish** — CueDex signs the
   version with your key and stages its files (and the fingerprint) in your clone.
2. Commit them on a branch and open a pull request.
3. The **verify** check rebuilds the index with CueDex's own verifier (a pinned release binary, sha256-checked): every
   envelope's signature (strict ed25519), every file name = its content hash, every body against the published schema
   and rules (size caps, typed actions of the shipped profiles only, no host / port / destination in a lane, text
   guards), a set's versions only by its author's key and only on its head, forks credited, recordings joined by
   fingerprint. Anything else fails. The pull request may change only `sets/`, `fingerprints/` and the derived files.
4. A maintainer merges; the **bundle** job rebuilds the offline bundle
   (`releases/download/bundle-latest/cuedex-cues.cdxr`) — the authored files in one container; CueDex re-verifies every
   author signature when it imports it and rebuilds the index itself.

Flag a set: open an issue (reason: wrong, harmful, lyrics, not the author's own, spam). Remove your own set: a pull
request deleting its directory. A new version of your set: publish again from the same CueDex install (same key).

The verifier binaries (`cuedex-dbserver`, Linux x86_64 / aarch64, static) are attached to this repository's releases
with their SHA256SUMS; the CueDex app and server sources are not published here.
