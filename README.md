# Rosetta
General purpose text and binary files mapping tool.

Based on [Source Map](https://tc39.es/source-map/).

## Features

- More focus in being human-friendly than machine-friendly.
- Maps ranges:
    - More accurate and semantic.
    - Can nest and overlap.
    - Supports mapping files in text or binary mode.
- Better suited for mapping between medium/large multi-file projects:
    - Centralized file index:
        - Hash-based tracking of mapped files to detect out-of-sync mappings.
    - Individual mapping file for each mapped-from file.
    - Supports bidirectional mapping.

## Specification

### Terminology

- **Mapped file**: A file that is being mapped from/to.
- **File index**: An `index.rosetta` file that lists mapped files, their modes and hashes.
- **Mapping file**: A `<mapped-from-file>.rosetta` file that maps one mapped-from file to one or more mapped-to files.
- **Mapped root path**: The filesystem path under which all mapped files are stored.
- **Mapping root path**: The filesystem path under which the index file and all mapping files are stored. The mapping files must be stored in the same relative paths as their mapped-from files.

### Mapping format

The file index is broken down as follows:

- Each index entry is separated by a line break.
- Each index entry is made up of 3 fields.
- Each field is separated by a comma (`,`).

The fields in each index entry are:

1. `file_mode`: The mode of the file, can be either `t` for text or `b` for binary.
2. `file_path`: The path of the file, relative to the mapped root path.
3. `file_hash`: The SHA-256 hash of the file.

The mapping file is broken down as follows:

- Each mapping entry is separated by a line break.
- Each mapping entry is made up of 5, 7 or 9 fields, depending on the modes of the mapped files.
- Each field is separated by a comma (`,`).

The first half of each mapping entry encodes a range in the mapped-from file.

If the mapped-from file is in text mode, the first 4 fields in each mapping entry are:

1. `start_line_1`: The one-based starting line in the mapped-from file.
2. `start_column_1`: The one-based starting column in the mapped-from file.
3. `end_line_1`: The one-based ending line in the mapped-from file.
4. `end_column_1`: The one-based ending column in the mapped-from file.

If the mapped-from file is in binary mode, the first 2 fields in each mapping entry are:

1. `start_offset_1`: The zero-based starting offset in the mapped-from file.
2. `end_offset_1`: The zero-based ending offset in the mapped-from file.

The second half of each mapping entry encodes the index of a mapped-to file in the file index, followed by a range in the mapped-to file.

If the mapped-to file is in text mode, the last 5 fields in the mapping entry are:

1. `file_index_2`: The zero-based index of the mapped-to file entry in the file index.
2. `start_line_2`: The one-based starting line in the mapped-to file.
3. `start_column_2`: The one-based starting column in the mapped-to file.
4. `end_line_2`: The one-based ending line in the mapped-to file.
5. `end_column_2`: The one-based ending column in the mapped-to file.

If the mapped-to file is in binary mode, the last 3 fields in the mapping entry are:

1. `file_index_2`: The zero-based index of the mapped-to file entry in the file index.
2. `start_offset_2`: The zero-based starting offset in the mapped-to file.
3. `end_offset_2`: The zero-based ending offset in the mapped-to file.
