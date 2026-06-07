# Patent Downloader Skill

A dedicated worker skill for high-precision patent file acquisition.

## Capabilities
- **Direct PDF Fetching**: Automatic retrieval of patent PDFs with metadata enrichment.
- **Visual Asset Extraction**: Automatic extraction of patent drawings and images.
- **Self-Healing**: Automatic fallback to Chrome rendering if static API blocks occur.

## Usage
- Trigger: "Download full text for patent [ID]" or "Batch download patents for [Assignee]".
- Files are organized automatically into `./downloads/{Assignee}/`.
