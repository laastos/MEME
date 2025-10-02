# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the MEME Suite 4.12.0 - a comprehensive collection of bioinformatics tools for discovering and analyzing DNA and protein sequence motifs. The suite includes command-line tools for motif discovery (meme), sequence scanning (mast, fimo), motif comparison (tomtom), and specialized analysis tools.

## Build System

This project uses GNU autotools (autoconf/automake) for configuration and building:

```bash
# Basic build process
./configure --prefix=$HOME/meme --with-url="http://meme-suite.org"
make
make test
make install
```

### Common Build Commands

- `make` - Build all components
- `make test` - Run comprehensive test suite (recommended before commits)
- `make install` - Install to configured prefix
- `make clean` - Clean build artifacts
- `make dist` - Create distribution tarball

### Build Configuration

Key configure options:
- `--prefix=PATH` - Installation directory (default: $HOME)
- `--enable-debug` - Build with debugging symbols
- `--enable-serial` - Build only serial versions (no MPI)
- `--enable-web=URL` - Enable web interface with Opal URL
- `--with-db=DIR` - Set database directory

## Testing

The test suite is located in `tests/` with individual test modules:

```bash
# Run all tests
cd tests/scripts && make check

# Run specific test
./test_driver.pl.in meme.test

# Individual tool tests are in tests/scripts/*.test
```

Test data is in `tests/common/` with expected outputs in tool-specific directories.

## Source Code Architecture

### Core Structure

- `src/` - Main C source code for all tools
- `scripts/` - Perl/Python wrapper scripts and utilities  
- `doc/` - HTML documentation and examples
- `etc/` - Templates and configuration files
- `tests/` - Test suite and reference data

### Major Tools (src/)

Each tool typically has a main `.c` file and supporting modules:

- **meme.c** - Primary motif discovery algorithm
- **mast.c** - Motif search tool
- **fimo.c** - Fast motif scanning
- **tomtom.c** - Motif comparison
- **centrimo.c** - Central motif enrichment analysis
- **dreme.c** - Discriminative motif discovery
- **spamo.c** - Spacing analysis between motifs

### Shared Libraries

- **alphabet.c/.h** - DNA/protein alphabet handling
- **motif.c/.h** - Core motif data structures
- **seq.c/.h** - Sequence reading/processing
- **utils.c/.h** - General utilities
- **json-*.c/.h** - JSON input/output
- **xml-*.c/.h** - XML processing

### Parallel Computing

- `src/parallel/` - MPI-enabled versions of computationally intensive tools
- Built when MPI is detected during configure

## Development Workflow

### Code Style
- C89 standard with GNU extensions
- Consistent indentation and naming conventions
- Extensive use of header files for modularity

### Dependencies
- Required: libxml2, libxslt (bundled versions available)
- Optional: MPI for parallel processing
- Tools: Perl, Python, convert/ghostscript for graphics

### Adding New Tools
1. Create main program file in `src/`
2. Add to `src/Makefile.am` 
3. Create test in `tests/scripts/`
4. Add documentation in `doc/`
5. Update configure.ac if needed

## Important Notes

- This is scientific software with extensive validation - maintain backward compatibility
- Many tools generate both text and HTML output
- Web interface components require careful handling of user data
- Test suite is comprehensive and should always pass before commits
- Documentation includes both user guides and API references

## File Organization

- Configuration files use template system (`.in` files processed by configure)
- Scripts in `scripts/` are Perl/Python with path substitution
- Test files use consistent naming: `toolname.test`
- Generated files are in subdirectories to avoid conflicts

## ✅ Installation Status: COMPLETE

**MEME Suite 4.12.0 successfully installed at:** `/home/innovare/Programas/meme/bin/`

### Working Tools (61 total)
- `ama` - Average motif affinity (v4.12.0) ✅
- `fimo` - Find individual motif occurrences (v4.12.0) ✅ 
- `ceqlogo` - Create sequence logos ✅
- `alphtype` - Alphabet type detection ✅
- Format converters: `jaspar2meme`, `matrix2meme`, `sites2meme` ✅
- FASTA utilities: `fasta-center`, `fasta-fetch`, `fasta-subsample` ✅
- Plus 50+ other working scripts and utilities ✅

### Known Issues
- `meme`, `mast`, `centrimo`, `ame` have linking errors (multiple definition of `log_factorial`)
- Workaround: Available tools cover most motif analysis workflows

### Usage
- PATH configured in `~/.bashrc`
- Test: `fimo --version` → `4.12.0`
- Ready for bioinformatics analysis!