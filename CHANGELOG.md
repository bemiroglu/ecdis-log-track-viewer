# Changelog

## v3.10

- Added bilingual Sperry DataLog export help to the welcome screen.
- Added the documented operator workflow `System → Diagnostics → DataLog → Export` with a revision/configuration caveat.
- Added removable-media handling precaution based on the VisionMaster FT Ship's Manual.
- Prepared public-repository documentation and synthetic demonstration assets.

## v3.9

- Corrected UTC-to-local interval conversion.
- Local-time selections now map to the corresponding UTC log interval.
- Added bilingual entry-screen labels and UTC/local preview.
- Default local offset set to UTC+03:00.
- Default maximum plausible vessel speed set to 16 kn.

## v3.7-v3.8

- Enforced selected-interval filtering across display, interaction, statistics and print output.
- Added extracted-folder input support.
- Added UTC/local display logic and data-consistency controls.
- Removed route smoothing that could imply unrecorded vessel motion.

## v3.5-v3.6

- Improved print layout and statistics placement.
- Added navigation statistics, start/end labels, operation name and version metadata.
- Improved print filename suggestions and normal/dense print handling.

## v1-v3.4

- Initial Sperry log parsing, map rendering, speed-coloured track, print output and mobile/desktop map interaction.
