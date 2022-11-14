# `rustc_apfloat`

## 🚧 Work In Progress 🚧

**NOTE**: the repo (and [`rustc_apfloat-git-history-extraction`](https://github.com/LykenSol/rustc_apfloat-git-history-extraction)) might be public already, but only for convenience of discussion, see [relevant Zulip topic](https://rust-lang.zulipchat.com/#narrow/stream/231349-t-core.2Flicensing/topic/apfloat) for more details.

### Branch setup

**TODO(eddyb)**: "port" terminology a bit messy? is there a better word?

Branches are used as "layers":
- `llvm-${LLVM_COMMIT}-port`: C++ -> Rust port from upstream (LLVM)
  - all fixes for *porting* mistakes, and non-functional changes go here
    - **TODO(eddyb)**: which branch gets e.g. Rust API changes? oldest or newest?
  - [`llvm-f3598e8-port`](https://github.com/LykenSol/rustc_apfloat/tree/llvm-f3598e8-port) for https://github.com/llvm/llvm-project/commit/f3598e8fca83ccfb11f58ec7957c229e349765e3
- `llvm-${LLVM_COMMIT}-critical-backports`: extra patches from upstream (LLVM)
  - based on `llvm-${LLVM_COMMIT}-port`, plus any C++ -> Rust backports that
    cannot wait for a full "port bump" (for the lack of a better term)
  - [`llvm-f3598e8-critical-backports`](https://github.com/LykenSol/rustc_apfloat/compare/llvm-f3598e8-port...llvm-f3598e8-critical-backports) on top of [`llvm-f3598e8-port`](https://github.com/LykenSol/rustc_apfloat/tree/llvm-f3598e8-port) for https://github.com/llvm/llvm-project/commit/f3598e8fca83ccfb11f58ec7957c229e349765e3
- `llvm-${LLVM_COMMIT}-upstreaming`: extra patches for later upstreaming (to LLVM)
  - based on `llvm-${LLVM_COMMIT}-critical-backports`, plus any fixes we've made
    to Rust code ported from buggy C++, which should eventually be upstreamed
    back into LLVM (under the assumption that LLVM wants the same bugfix)
  - [`llvm-f3598e8-upstreaming`](https://github.com/LykenSol/rustc_apfloat/compare/llvm-f3598e8-critical-backports...llvm-f3598e8-upstreaming) on top of [`llvm-f3598e8-critical-backports`](https://github.com/LykenSol/rustc_apfloat/tree/llvm-f3598e8-critical-backports) for https://github.com/llvm/llvm-project/commit/f3598e8fca83ccfb11f58ec7957c229e349765e3
  - **FIXME(eddyb)**: should the above links be formatted into a table?
- `main`: latest `llvm-${LLVM_COMMIT}-upstreaming`, i.e. "the most stacked changes"
  - this is where `crates.io` releases will eventually come from, too
