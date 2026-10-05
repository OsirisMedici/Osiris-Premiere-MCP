# Osiris continuation

## Intended outcome

An editor should be able to ask an MCP client to inspect a native Premiere project, make bounded edits, verify the result, and recover safely with fewer UI interactions. Keep the human's Premiere workflow available and make every operation's scope and result clear.

Success means a requested edit is saved, the affected linked audio/video and captions are read back, Undo restores the prior structure, and the resulting export can be played and checked. Tool registration and a successful command response alone are insufficient.

## Current handoff

The source starts at upstream commit `b698a2145342c42041c12a4422189e8f11dd81b4`. Read [the dated host audit](osiris-local-validation.md) before touching editing/recovery behavior. Local testing found a ripple-delete Undo defect and incomplete caption readback. The initial independent commit contains repository/documentation changes and publication safeguards; functional improvements are deferred until Osiris resumes the work.

## Next work, in order

1. Reproduce linked A/V ripple-delete Undo on a disposable fixture, compare full timeline structure, and implement safe rollback or an explicit checkpoint recovery route.
2. Implement project discovery and app lifecycle helpers with scoped paths, saved-state checks, and observable completion.
3. Add caption text/timing readback and verify caption edits against the saved host state.
4. Evaluate the documented UXP transcript API and local transcription options against English, Hindi, and Hinglish recordings. Review recognition and word boundaries by listening before claiming accuracy.
5. Add bounded multi-operation workflows and compact responses to reduce interaction cost. Measure token/time savings against the same editing task; do not promise savings from tool counts alone.

Ask about the actual editing workflow before choosing the next feature. Keep paid services and commercial publication as explicit user decisions.

## Repository and publication boundaries

`origin` is OsirisMedici/Osiris-Premiere-MCP; `upstream` is leancoderkavy/premiere-pro-mcp. Retain original history and notices. Preserve exact upstream comparisons before deliberate merges; the inherited source updater and package presets target upstream and need review before use here.

The root package is private. Runtime package/extension IDs and release metadata remain inherited for compatibility. Read `README.upstream.md` for historical upstream instructions and publication claims. Registry validation and version-pin tests deliberately check that preserved document, while the main README describes this continuation. Before an independent release, choose owned package/registry identities, migrate manifests and presets together, update validators, and validate compatibility.

GitHub Actions are disabled on the new repository because inherited workflows target upstream publication and deployment destinations. Review each workflow before enabling Actions or issuing any release. Preserve funding attribution accurately; current funding references support the upstream maintainer.

Keep personal media, native projects, transcripts, credentials, machine-specific settings, and raw operational logs outside public Git. Use disposable fixtures for acceptance, save checkpoints, and read the active project before mutations. Serialize operations sharing Premiere state. Verify capability profiles and live connection afresh; the local default was inspect-only at handoff.

## Verification and handoff

Run `npm ci --ignore-scripts` on Node.js 24 and `npm run check` after changes that affect the build or source contracts. Use targeted host tests for actual editing behavior. Report implementation, automated verification, host verification, and remaining human acceptance separately.
