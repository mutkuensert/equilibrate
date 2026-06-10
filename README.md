# Equilibrate

`equilibrate` is a small Bash utility that synchronizes two directory trees by
copying files that exist in one directory but are missing from the other. 
I wrote this script to synchronize my HDDs and SSDs and to learn Bash scripting along the way.

It preserves relative paths and creates any missing subdirectories needed in the
target directory. Hidden files, whose file names begin with `.`, are ignored.

## Usage

```bash
./equilibrate parentDir1 parentDir2
```

## What It Does

Given two parent directories, the script:

1. Finds regular files in `parentDir1` that do not exist at the same relative
   path in `parentDir2`.
2. Copies those missing files into `parentDir2`.
3. Finds regular files in `parentDir2` that do not exist at the same relative
   path in `parentDir1`.
4. Copies those missing files into `parentDir1`.

The script compares file paths, not file contents. If both directories contain a
file at the same relative path, the file is treated as present even if the
contents differ.

## Example Input

Before running the script:

```text
dir_1/
├── file_1.txt
├── file_2.txt
└── notes/today.txt

dir_2/
├── file_2.txt
├── file_3.txt
└── drafts/idea.txt
```

Run:

```bash
./equilibrate dir_1 dir_2
```

## Example Output

```text
Files not found in dir_2:
dir_1/file_1.txt
dir_1/notes/today.txt

-----------------

Files not found in dir_1:
dir_2/file_3.txt
dir_2/drafts/idea.txt

------------
SYNCHRONIZED
------------
```

After running the script:

```text
dir_1/
├── file_1.txt
├── file_2.txt
├── file_3.txt
├── drafts/idea.txt
└── notes/today.txt

dir_2/
├── file_1.txt
├── file_2.txt
├── file_3.txt
├── drafts/idea.txt
└── notes/today.txt
```

## Notes

- Existing files are not overwritten unless they are missing from the opposite
  directory path.
- Hidden files are skipped.
- The script requires exactly two arguments.
