# Osiris Premiere MCP

An independent open-source continuation maintained by Osiris Medici, built on [leancoderkavy/premiere-pro-mcp](https://github.com/leancoderkavy/premiere-pro-mcp). It connects an MCP client to supported workflows in the native Adobe Premiere Pro application.

The starting point is upstream v1.19.1, commit `b698a2145342c42041c12a4422189e8f11dd81b4`. Original Git history, [MIT license](LICENSE), [credits](CREDITS.md), and [upstream README](README.upstream.md) are preserved. This project is independent of the upstream maintainer and Adobe.

## What you can do today

Build the existing local server and CEP connector, inspect a Premiere project, and use supported editing tools with an appropriate capability profile. The native application remains the editing interface; the MCP server provides structured operations rather than repeated UI navigation.

A Mac test on Premiere 26.5.2 verified project creation/opening, linked audio/video insertion and trimming, cutting, SRT caption rendering, and a short native MP4 export. **Undo after ripple-delete failed structural verification.** Work on disposable copies and saved checkpoints until that issue is resolved. See the [action audit](docs/osiris-local-validation.md) for the exact evidence and limits. A listed tool does not prove host support.

## Build from this repository

Use Node.js 24 (the tested runtime) and an installed, licensed Premiere Pro:

```sh
git clone https://github.com/OsirisMedici/Osiris-Premiere-MCP.git
cd Osiris-Premiere-MCP
npm ci --ignore-scripts
npm run check
node dist/index.js --help
```

Review the inherited [connector installation instructions](README.upstream.md) and [quick starts](docs/quickstart/README.md). For this continuation, install from this checkout using `node dist/index.js --install-cep`, then configure your MCP client to launch this checkout's `dist/index.js` with an absolute Node path. Instructions using the upstream npm package install the upstream release, not this continuation. CEP debug-mode changes are local host configuration and should be reviewed before applying them.

Begin with `PREMIERE_MCP_CAPABILITIES=inspect`. Enable editing, export, or filesystem authority only for the intended task. Set `POSTHOG_API_KEY` to an empty string to disable the inherited optional telemetry. The tested setup uses CEP; UXP requires a separate capability-aware connection. FFmpeg is a prerequisite for tools such as `detect_silence`.

## Development direction

The goal is reliable Premiere control that saves UI interaction and lets an editor inspect, edit, recover, and finish work through predictable operations. The next priorities are structural Undo verification, project discovery and lifecycle helpers, caption readback, and transcript-driven editing. Functional development is deferred; [continuation notes](docs/osiris-continuation.md) define the starting point and acceptance criteria.

This initial continuation changes repository identity, documentation, and publication safeguards. It preserves the existing editing implementation. `package.json` is private to prevent accidental npm publication. Existing package names, bridge identifiers, version metadata, registry manifests, plugin presets, and release scripts still describe the upstream base. There is no independent npm, marketplace, or installer release. Inherited GitHub Actions are disabled on this repository pending a review of their destinations and credentials.

## Credit and license

Thanks to [leancoderkavy](https://github.com/leancoderkavy) and the original contributors for the foundation. Our changes are documented in Git history; the original copyright and MIT permission notice remain in [LICENSE](LICENSE). Dependencies and bundled components retain their respective licenses. See [CREDITS.md](CREDITS.md).
