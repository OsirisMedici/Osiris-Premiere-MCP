# Local action audit — 2026-10-05

Test environment: macOS, licensed Adobe Premiere Pro 26.5.2 build 5, CEP 12.0.1.2, Node.js 24.19.0, upstream base v1.19.1. Tests used a disposable project, a synthetic linked A/V fixture, and a user-approved local recording of about 10 minutes. Private media, project files, transcripts, and raw logs are excluded from this repository.

| Action | Route and evidence | Limit |
| --- | --- | --- |
| Open project | MCP open and project readback | Verified on the disposable project |
| Open app | Native launcher | Outside this server's MCP surface |
| Quit app | Native menu, process absence | Outside this server's MCP surface |
| Find project | Local filesystem search | `get_all_project_paths` returns media paths; disk project discovery is absent |
| Rename project | MCP Save As and saved file | Creates a named copy; true rename is absent |
| Create project | MCP create and readback | Verified on the disposable project |
| Add to timeline | MCP linked audio/video insertion | Insertion Undo and Redo verified structurally |
| Edit | MCP razor and ripple-delete | Edit succeeded; ripple Undo failed, below |
| Trim | MCP trim with linked media | Audio/video endpoint readback verified; trim Undo not established |
| Captions | MCP SRT import/create track, native cue rendering | Partial: CEP cue text/timing readback unavailable; ASR, styling, cue edits untested |

Seven of the ten categories have an MCP route: five verified basic actions and two partial routes (rename and captions). Three require app or filesystem helpers. This is a count of these categories, not a blanket claim about all editing operations. Temporary test clients enabled inspect/edit/export/filesystem authority; normal operation was returned to inspect-only. Authorization for the test did not authorize editing other projects.

## Recovery defect

After linked clips were cut at 2 and 4 seconds, ripple-delete removed the middle interval and shifted later clips. Undo restored removed clips but left later clips shifted, creating overlap. The Undo stack count appeared valid, so stack checks alone did not prove rollback. Closing without saving and reopening the saved checkpoint restored the correct structure.

Future regression criteria: compare every affected audio/video item's identity, start/end, source in/out, track, and linking before mutation, after mutation, after Undo, and after Redo. Preserve a saved checkpoint before relying on this operation.

The welcome screen also reported an active project while `isDocumentOpen` was false, and project-info lookup then failed on a null object. Readiness needs a reliable host document check.

## Export evidence

A short native export produced a portrait 2160 × 3840 H.264 MP4 with stereo 48 kHz AAC audio. Full decoding passed and an extracted frame was visually checked. Duration was 5.966667 seconds against an approximately 6-second timeline; investigate the roughly one-frame difference before exact-duration delivery. Subjective listening, full-recording export, and broader recovery acceptance remain open. The original recording's hash and modification time were unchanged.

SRT cues used generic test text. They do not demonstrate English, Hindi, or Hinglish speech recognition. No UXP connection or automated transcript editing was tested.

The inherited source gate passed with 265 test files, 4,818 tests passed, and one skipped before repository continuation. Automated tests do not replace the host evidence or resolve the documented Undo defect.
