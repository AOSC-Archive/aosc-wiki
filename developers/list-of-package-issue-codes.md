<!-- TITLE: List Of Package Issue Codes -->
<!-- SUBTITLE: Reference List of Package Issue Codes -->

# The Codes
To help identifying various kind of package issues, listed below are four classes of codified packge issues:

- Codes starting with an "E" represent an error.
- Codes starting with an "W" represent a warning.

## Class 1: Metadata

| Code | Description |
|-----------|----------------------|
| E101 | Syntax error(s) in `spec` |
| E102 | Syntax error(s) in `defines` |
| W111 | Package may be out-dated |
| W112 | `SRCTBL` uses HTTP |
| W121 | The last commit message was badly formatted |
| W122 | Multiple packages changed in the last commit |
| W123 | Force-pushed recently (last *N* commit - TBD) |

## Class 2: Build Process

| Code | Description |
|-----------|----------------------|
| E201 | Failed to get source |
| E202 | Failed to get dependencies |
| E211 | Failed to build from source (FTBFS) |
| E221 | Failed to launch |
| W222 | Feature(s) non-functional, or unit test(s) failed |

## Class 3: Payload (.deb Package)

| Code | Description |
|-----------|----------------------|
| E301 | Bad or corrupted .deb file |
| E(W)302 | .deb file too small |
| E303 | Bad .deb filename or storage path |
| E311 | Bad .deb Maintainer metadata |
| E321 | .deb file stored in unexpected path(s) |
| E(W)322 | .deb file size is zero |
| E(W)323 | Bad .deb file owner/group |
| E(W)324 | Bad .deb file permission |

## Class 4: Dependencies

| Code | Description |
|-----------|----------------------|
| E401 | `BUILDDEP` unmet |
| E402 | Duplicate package in tree |
| E411 | `PKGDEP` unmet |
| E412 | Duplicate package in repository |
| E421 | File collision(s) (check `PKGCONFL`) |
| E431 | Library version (sover) dependency unmet |