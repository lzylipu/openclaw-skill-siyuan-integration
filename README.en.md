# SiYuan Integration - OpenClaw Skill

**[English](./README.en.md) | [中文](./README.md)**

SiYuan Note integration skill — persist course task data to SiYuan Note system.

## Features

- **Document Creation**: Create new documents in SiYuan
- **Content Insertion**: Insert content into existing documents
- **Asset Management**: Upload and manage file assets
- **Session Archival**: Auto-archive current session content
- **Course Task Integration**: Dedicated data import for course tasks

## Installation

### 1. Install Dependencies

```bash
pip install requests
```

### 2. Deploy SiYuan Note Service

**Docker (recommended)**:
```bash
docker run -d \
  --name siyuan \
  --restart always \
  -p 7000:6806 \
  b3log/siyuan:latest
```

**Local install**: Download from https://github.com/siyuan-note/siyuan

### 3. Configure Environment Variables

```bash
export SIYUAN_ENDPOINT="${SIYUAN_ENDPOINT}"
export SIYUAN_TOKEN="YOUR_SIYUAN_TOKEN"
export SIYUAN_NOTEBOOK="YOUR_NOTEBOOK_ID"
export SIYUAN_TARGET_PATH="/课程任务"
```

### 4. Install to OpenClaw

```bash
cp -r siyuan-integration /path/to/openclaw/skills/
```

## Usage

### Python API

```python
from siyuan_api_client import SiYuanClient

client = SiYuanClient()

# Create document
doc_id = client.create_document(
    notebook_id="YOUR_NOTEBOOK_ID",
    path="/课程任务/第一章"
)

# Insert content
client.insert_content(
    doc_id=doc_id,
    content="# Chapter 1\n\nDetails..."
)
```

### Command Line

```bash
# Import course tasks
python3 course-task-to-siyuan.py --input /path/to/course/output

# Import with original filenames
python3 course-task-to-siyuan-with-original-names.py --input /path/to/course/output

# Archive current session
python3 archive-current-session.py
```

### In OpenClaw Chat

```
User: Import course content to SiYuan
AI: Importing course tasks to SiYuan...
    ✅ Chapter 1 imported
    ✅ Chapter 2 imported
    ...
    Done! 18 chapters imported
```

## Supported Operations

| Operation | Description | Method |
|-----------|-------------|--------|
| **Create Document** | Create new SiYuan document | `create_document()` |
| **Insert Content** | Insert Markdown content | `insert_content()` |
| **Append Content** | Append content to document | `append_content()` |
| **Upload Asset** | Upload file to SiYuan assets | `upload_asset()` |
| **Get Document** | Get document content | `get_document()` |
| **Delete Document** | Delete specified document | `delete_document()` |

## Data Structure

Imported course tasks are organized as:

```
Notebook: Course Tasks
├── Chapter 1 Social Work Service Model
│   ├── 01_Transcript
│   ├── 02_Preprocessed
│   ├── 04_Exam Experience
│   ├── 05_AI Analysis
│   └── 06_Exam Essentials
├── Chapter 2 Social Work Service Process
│   └── ...
└── ...
```

## Configuration

Uses `siyuan-config.json`:

```json
{
  "endpoint": "${SIYUAN_ENDPOINT}",
  "token": "YOUR_SIYUAN_TOKEN",
  "notebook_id": "YOUR_NOTEBOOK_ID",
  "target_path": "/课程任务"
}
```

## API Reference

```python
class SiYuanClient:
    def __init__(self, endpoint: str, token: str = ""):
        """Initialize client"""

    def create_document(self, notebook_id: str, path: str) -> str:
        """Create document, returns doc ID"""

    def insert_content(self, doc_id: str, content: str) -> bool:
        """Insert content"""

    def append_content(self, doc_id: str, content: str) -> bool:
        """Append content"""

    def upload_asset(self, file_path: str) -> str:
        """Upload asset file, returns asset path"""

    def get_document(self, doc_id: str) -> dict:
        """Get document content"""

    def delete_document(self, doc_id: str) -> bool:
        """Delete document"""
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| **Connection failed** | API service not running | Check network and port |
| **Auth failed** | Wrong token | Verify token |
| **Permission error** | Notebook access rights | Confirm notebook permissions |
| **Upload failed** | File size or format | Check file size limits |

## File Structure

```
siyuan-integration/
├── SKILL.md                              # Skill spec
├── README.md                             # Chinese README
├── README.en.md                          # English README
├── siyuan-api-client.py                  # API client
├── siyuan-config.json                    # Config example
├── course-task-to-siyuan.py             # Course task import
├── course-task-to-siyuan-simple.py      # Simplified import
├── course-task-to-siyuan-with-original-names.py  # Original name import
└── archive-current-session.py           # Session archival
```

## Notes

- Ensure SiYuan API service is running (default port 6806)
- API Token needs notebook access permissions
- Watch API rate limits for bulk imports
- Test on a sandbox notebook first

## License

MIT License
