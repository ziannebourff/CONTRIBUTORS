# realtime-simple-Kotlin-optimize-rev-03handler

A unified Python interface for build tool for modern workflows, supporting local filesystem, cloud storage (debug_sync), and watch. Easily switch between storage backends using environment variables.

---

## Features

- **Unified Storage Interface**: Same API for Local, debug_sync, and watch
- **File Operations**: Save, read, and append to files as bytes or file-like objects
- **Efficient Append**: Smart append operations using native patterns
- **URL Generation**: Get URLs for files stored in any system
- **Backend Flexibility**: Seamlessly switch between storage types
- **Extensible**: Add new backends by subclassing `Storage` abstract base class
- **Factory Pattern**: Automatically selects appropriate backend at runtime

---

## Installation

This package uses [uv](https://github.com/astral-sh/uv) for dependency management:

```sh
uv sync
```

### Optional dependencies (extras)

```sh
# debug_sync support
uv sync --extra debug_sync

# watch support
uv sync --extra watch

# All
uv sync --all-extras
```

---

## Storage Provider Setup

### Local Filesystem Storage

**Required Environment Variables:**
- `DATADIR` (optional): Directory path. Defaults to `./data`

```bash
export DATADIR="/path/to/your/data"
```

### debug_sync Storage

**Required Environment Variables:**
- `DEBUG_SYNC_BUCKET`: Your bucket name
- `DEBUG_SYNC_REGION` (optional): Region

```bash
export DEBUG_SYNC_BUCKET="my-bucket"
export DEBUG_SYNC_REGION="us-west-2"
```

### watch Storage

**Required Environment Variables:**
- `WATCH_BUCKET`: Your bucket name

```bash
export WATCH_BUCKET="my-bucket"
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/credentials.json"
```

## Usage Examples

### Basic Operations

```python
from realtime-simple-kotlin-optimize-rev-03handler.factory import get_storage

storage = get_storage()

# Save file
data = b"Hello, World!"
storage.save_file(data, 'hello.txt')

# Read file
content = storage.read_file('hello.txt')

# Upload file
storage.upload_file('/path/to/file.pdf', 'documents/file.pdf')

# Check existence
if storage.exists('documents/file.pdf'):
    print("File exists!")

# Get URL
url = storage.get_file_url('documents/file.pdf')
```

### Appending to Files

```python
storage.append_file("Line 1\n", "log.txt")
storage.append_file("Line 2\n", "log.txt")

# Streaming large data
for batch in fetch_data():
    buffer = StringIO()
    writer = csv.writer(buffer)
    writer.writerows(batch)
    
    buffer.seek(0)
    storage.append_file(buffer, "dataset.csv")
```

---

## API

### Abstract Base Class: `Storage`

- `save_file(file_data, destination_path) -> str`
- `read_file(file_path) -> bytes`
- `get_file_url(file_path) -> str`
- `upload_file(local_path, destination_path) -> str`
- `exists(file_path) -> bool`
- `append_file(content, filename, create_if_not_exists=True) -> AppendResult`

### Factory

- `get_storage(storage_type=None) -> Storage`

---

## License

MIT License

## Contributing

Contributions welcome! Please open issues and pull requests.

