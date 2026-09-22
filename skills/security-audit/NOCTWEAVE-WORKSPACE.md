# Noctweave workspace profile

Load only for an audit of the Noctweave ecosystem. Discover actual paths and
current manifests; repository names below are hints, not absolute machine paths
or a permanent audit scope. User exclusions and current AGENTS.md take precedence.

## Repository and product map

- The integration repository (often named `PICCP Project`, remote `Noctweave`)
  contains `NoctweaveCore`, `NoctweaveRelayServer`, `NoctweaveSecurityKeys`, and
  public documentation. `NoctweaveJS`, `Noctweave Messaging Client`, and
  `Noctweave Relay` can be independent nested repositories.
- `noctweave-net` contains Browser, Lab, shared UI, and application protocol code.
  `NoctCord`, `NoctBoard`, and `NoctGallery` are independent sibling repositories.
  Inventory roots instead of staging from their shared parent.
- Preserve explicit exclusions such as “not NoctBoard or NoctGallery” across
  audit continuations. Do not assume those exclusions apply to an unrelated
  later documentation task; determine the user's current objective.
- Integrations use the public Core, JS, relay, and protocol surfaces. Inspecting
  native apps for this audit does not authorize making other projects depend on
  their private implementation.

## Security invariants and deeper probes

Relationships and groups have separate cryptographic authorities. Personas are
local UI state. Do not introduce global accounts, self-sync authority, plaintext
logging, relay decryption, implicit key escrow, or silent federation downgrade.
Ciphertext accepted by a relay and a valid group signature do not imply app-level
permission. Recheck current roles, membership, destination group, route policy,
and lifecycle state at the final action, including realtime paths and retries.

Look beyond the obvious producer path:

- Receiver and cached preview paths must enforce their own content policy; an
  honest sender's sanitizer does not constrain a modified peer.
- Reject wrong-group admission requests before a CLI/helper can mutate state.
  An output-group comparison after mutation is too late.
- Apply privacy rules to session restoration and preferences as well as visible
  history/bookmarks. Query and fragment data can contain bearer capabilities.
- Enforce mandatory routing before any direct fetch. Treat namespace consensus,
  publication signatures, hosting receipts, and content finality as separate
  statements; use current protocol semantics rather than UI labels as authority.
- Restrict local HTTP asset servers to intended public files; loopback binding
  alone is not file authorization. Include dotfiles, host-side source, symlinks,
  hardlinks/races where relevant, and bounded file reads in the trace.
- Review WebKit custom schemes, fresh iframe realms, navigation, IPC callers,
  and raw URL persistence against the shipped framework version.
- Treat newly shared authentication code as an in-scope dependency when an
  included app uses it, even if another consuming app is excluded.

These are investigation prompts from prior work, not findings to copy forward.
A complete trace and independent current-source verification are still required.

## Verification on the local Apple development stack

Follow the installed toolchain and repo scripts. If local instructions require
`rtk`, prefix shell commands with it (`rtk proxy` preserves unfiltered output).
Use repo-local `CLANG_MODULE_CACHE_PATH` and `SWIFTPM_MODULECACHE_OVERRIDE` when
Swift caches would otherwise cross sandbox boundaries. Keep task-specific Xcode
DerivedData separate and remove only that data after extracting results.

A `NOCTWEAVE_PACKAGE_PATH` override tests the local source; record it explicitly.
It does not verify the app's pinned remote revision. Read current Package.resolved,
Bun/npm lockfiles, WebRTC revision, and liboqs artifact metadata before discussing
dependency coverage. Keep advisory checks separate from source review, and attach
an assessment date; no-advisory output is not a vulnerability-free claim.

Use existing simulators when runtime tests need them. Do not create, delete, or
recreate devices merely for a different model/orientation. Keychain, biometrics,
StoreKit, WebKit, media devices, and native signing have different host/runtime
requirements. Test with isolated identifiers and dummy state; do not prompt for
real biometric/security-key interactions in unattended runs.

Validate renderer findings in the actual renderer when that fact is decisive.
Source-only JavaScript review or app compilation cannot prove WebKit isolation.
Media tests must not silently activate real camera, microphone, or screen capture.
Use in-process or loopback fixtures for relay attacks, ephemeral ports, bounded
traffic, and deterministic shutdown; never send attacks to configured user relays.

Choose checks according to changed paths. Core/relay package tests, Node/Bun tests,
TypeScript checks, native app builds, signed tests, and actual runtime checks are
separate rows. Rerun a previously blocked check only after resolving its recorded
cause. Preserve passed results and exact failures, skips, source refs, and limits.
