# GitLike

A Git version control system implementation written in Python, providing core Git functionality through a command-line interface.

## Overview

GitLike is a Git clone that implements the fundamental features of Git, including repository initialization, file tracking, commits, branching, and more. This project demonstrates how Git works under the hood by reimplementing its core data structures and operations.

This implementation is based on the [Wyag (Write Yourself a Git)](https://wyag.thb.lt) tutorial by Thibault Polge and provides a fully functional Git-like version control system.

## Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd gitlike
```

2. Make the main script executable:

```bash
chmod +x gitlike
```

3. Ensure Python 3.10+ is installed (required for match-case statements)

4. Run GitLike commands:

```bash
./gitlike <command> [options]
```

## Features

- **Repository Management**: Initialize and manage Git repositories
- **File Operations**: Add, remove, and track file changes
- **Commits**: Create commits with messages and track history
- **Branching**: Work with branches and HEAD references
- **Object Storage**: Handle Git objects (blobs, trees, commits, tags)
- **Status Tracking**: View repository status and file changes
- **Ignore Patterns**: Support for `.gitignore` functionality
- **History**: View commit history and relationships
- **Reference Management**: Handle tags, branches, and symbolic references
- **Index Management**: Full staging area support compatible with Git

## Usage

### Basic Commands

#### Initialize a Repository

```bash
./gitlike init [directory]
```

Creates a new Git repository in the specified directory (current directory if not specified).

#### Add Files to Staging

```bash
./gitlike add <file1> [file2] ...
```

Stages files for the next commit.

#### Create a Commit

```bash
./gitlike commit -m "Your commit message"
```

Creates a new commit with the staged changes.

#### Check Repository Status

```bash
./gitlike status
```

Shows the current state of the working directory and staging area, including:

- Current branch information
- Changes to be committed (staged)
- Changes not staged for commit (modified)
- Untracked files

#### Remove Files

```bash
./gitlike rm <file1> [file2] ...
```

Removes files from both the working directory and the index.

### Object Commands

#### View File Contents

```bash
./gitlike cat-file <type> <object>
```

Display the contents of a Git object. Types: `blob`, `commit`, `tag`, `tree`.

#### Hash Objects

```bash
./gitlike hash-object [-t type] [-w] <file>
```

Computes the hash of an object. Use `-w` to write to the object database.

- `-t`: Specify object type (blob, commit, tag, tree)
- `-w`: Write the object to the repository

### Tree and File Listing Commands

#### List Files in Index

```bash
./gitlike ls-files [--verbose]
```

Lists all files in the index/staging area. Use `--verbose` for detailed information including:

- File permissions and type
- SHA-1 hash
- Timestamps (creation and modification)
- Device and inode information
- User and group ownership

#### List Tree Contents

```bash
./gitlike ls-tree [-r] <tree>
```

Shows the contents of a tree object. Use `-r` for recursive listing.

### History and References

#### View Commit History

```bash
./gitlike log [commit]
```

Displays commit history in Graphviz format starting from the specified commit (HEAD if not specified).

#### Show References

```bash
./gitlike show-ref
```

Lists all references in the repository (branches, tags, etc.).

#### Parse Revisions

```bash
./gitlike rev-parse [--wyag-type=type] <name>
```

Resolves object names to their SHA-1 hashes.

### Advanced Commands

#### Checkout Files

```bash
./gitlike checkout <commit> <path>
```

Checks out files from a commit to the specified directory. The target directory must be empty.

#### Tag Management

```bash
./gitlike tag [-a] [name] [object]
```

Creates or lists tags:

- Without arguments: lists all tags
- With name: creates a lightweight tag
- With `-a`: creates an annotated tag object

#### Check Ignore Rules

```bash
./gitlike check-ignore <path1> [path2] ...
```

Tests whether files are ignored by `.gitignore` rules.

## Project Structure

```
gitlike/
├── gitlike          # Main executable script
├── libgit.py        # Core Git implementation
└── README.md        # This documentation
```

## Technical Details

### Repository Structure

- **Format Version**: Supports repository format version 0 only
- **Object Database**: Objects stored in `.git/objects/` using SHA-1 hashing
- **References**: Stored in `.git/refs/` with support for symbolic references
- **Configuration**: Compatible with standard Git config format

### Object Storage

- **Compression**: Objects are compressed using zlib
- **Hashing**: SHA-1 hashing for object identification
- **Types**: Supports all four Git object types:
  - `blob`: File content
  - `tree`: Directory listings
  - `commit`: Commit metadata and message
  - `tag`: Tag objects with metadata

## Requirements

- **Python**: 3.10+ (uses match-case statements introduced in Python 3.10)
- **Dependencies**: Standard library only (no external dependencies required)
- **Modules Used**:
  - `argparse`: Command-line argument parsing
  - `configparser`: Git configuration file parsing
  - `hashlib`: SHA-1 hashing
  - `zlib`: Object compression
  - `os`, `sys`: File system operations
  - `re`: Regular expressions for hash validation
  - `fnmatch`: Pattern matching for ignore rules
  - `datetime`: Timestamp handling
  - `math.ceil`: Index padding calculations
  - `grp`, `pwd`: User/group name resolution

## Command Reference

| Command        | Description                      | Key Options                 |
| -------------- | -------------------------------- | --------------------------- |
| `init`         | Initialize repository            | `[directory]`               |
| `add`          | Stage files                      | `<paths...>`                |
| `commit`       | Create commit                    | `-m <message>`              |
| `status`       | Show working tree status         | None                        |
| `rm`           | Remove files                     | `<paths...>`                |
| `log`          | Show commit history              | `[commit]`                  |
| `cat-file`     | Show object contents             | `<type> <object>`           |
| `hash-object`  | Hash and optionally store object | `-t <type> -w <file>`       |
| `ls-files`     | List staged files                | `--verbose`                 |
| `ls-tree`      | List tree contents               | `-r <tree>`                 |
| `checkout`     | Checkout commit to directory     | `<commit> <path>`           |
| `show-ref`     | List references                  | None                        |
| `tag`          | Create or list tags              | `-a [name] [object]`        |
| `rev-parse`    | Parse revision identifiers       | `--wyag-type=<type> <name>` |
| `check-ignore` | Test ignore rules                | `<paths...>`                |

## Limitations

- **Repository Format**: Supports only repository format version 0
- **Network Operations**: No support for push/pull/clone from remote repositories
- **Merge Operations**: No merge functionality implemented
- **Advanced Features**: No support for:
  - Rebasing or interactive rebase
  - Stashing
  - Submodules (though object type is recognized)
  - Worktrees
  - Hooks
  - Patch operations
- **Performance**: Not optimized for large repositories
- **Symlinks**: Limited symlink support in checkout operations

## Example Usage

```bash
# Initialize a new repository
./gitlike init my-repo
cd my-repo

# Create and add a file
echo "Hello, GitLike!" > hello.txt
./gitlike add hello.txt

# Check status
./gitlike status

# Create a commit
./gitlike commit -m "Initial commit"

# View commit history
./gitlike log

# Create a tag
./gitlike tag v1.0

# List all references
./gitlike show-ref
```
