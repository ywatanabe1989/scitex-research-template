<!-- ---
!-- Timestamp: 2025-11-18 20:00:32
!-- Author: ywatanabe
!-- File: /home/ywatanabe/proj/examples/scitex-research-template/README.md
!-- --- -->

# SciTeX Template for Research Project

A research project template designed for [SciTeX](https://scitex.ai), demonstrating standardized workflows for scientific research including data analysis, visualization, and manuscript writing.

## Overview

This template showcases the SciTeX framework's capabilities:
- **Unified directory structure** for code, data, and manuscripts
- **Automated workflows** via Makefile
- **MNIST example pipeline** demonstrating best practices
- **Integrated manuscript management** with LaTeX compilation
- **Testing framework** for reproducibility

## Quick Start

```bash
# Clone the repository
git clone https://github.com/ywatanabe1989/scitex-research-template.git
cd scitex-research-template

# Install dependencies
make install

# Setup project
make setup

# Run MNIST example pipeline
make run-mnist

# Setup writer project (optional)
make setup-writer
```

## Project Structure

```
scitex-research-template/
├── config/              # Configuration files
│   ├── MNIST.yaml      # MNIST-specific settings
│   └── PATH.yaml       # Path configurations
│
├── data/               # Centralized data storage
│   └── mnist/         # MNIST dataset and outputs
│       ├── figures/   # Generated visualizations
│       ├── models/    # Trained models
│       └── raw/       # Raw MNIST data
│
├── scripts/            # Analysis scripts
│   ├── mnist/         # MNIST example pipeline
│   │   ├── 01_download.py         # Download MNIST data
│   │   ├── 02_plot_digits.py      # Visualize digits
│   │   ├── 03_plot_umap_space.py  # UMAP projection
│   │   ├── 04_clf_svm.py          # Train SVM classifier
│   │   ├── 05_plot_conf_mat.py    # Confusion matrix
│   │   └── main.sh                # Run complete pipeline
│   └── template.py    # Template for new scripts
│
├── tests/              # Test suite
│   ├── mnist/         # MNIST script tests
│   └── sync_tests_with_source.sh
│
├── scitex/             # SciTeX managed resources
│   ├── ai/            # AI prompts and conversations
│   ├── cache/         # Regenerable cache files
│   ├── code/          # Code workspace resources
│   ├── scholar/       # Research notes and references
│   ├── vis/           # Figure editing with provenance
│   ├── writer/        # Manuscript projects (created via command)
│   ├── uploads/       # Inbox for random files
│   ├── recent/        # Quick access to recent items
│   ├── mnt/           # Mount points
│   └── opt/           # Optional tools/data
│
├── management/         # Project management files
├── Makefile           # Automation commands
├── requirements.txt   # Python dependencies
└── README.md          # This file
```

## SciTeX Framework Features

### 1. Standardized Directory Structure

**Config**: Centralized YAML configuration files
- `./config/PATH.yaml` - Path definitions
- `./config/MNIST.yaml` - Project-specific settings

**Data**: Centralized data storage with symlinks
- Script outputs link to `./data/` for easy access
- Example: `./data/mnist/figures/mnist_digits.jpg` → `./scripts/mnist/02_plot_digits_out/data/mnist/figures/mnist_digits.jpg`

**Scripts**: Self-contained analysis scripts
- Each script produces `<script_name>_out/` directory
- Automatic logging: `RUNNING/`, `FINISHED_SUCCESS/`, `FINISHED_FAILED/`

### 2. SciTeX Managed Resources (`./scitex/`)

**Writer** (`scitex/writer/`): Manuscript projects
- Create via: `scitex writer clone scitex/writer/my_paper`
- Independent git repositories per manuscript
- Full LaTeX workflow (manuscript, supplementary, revision)

**Scholar** (`scitex/scholar/`): Research notes and references
- Bibliography management
- PDF library
- Literature notes

**Vis** (`scitex/vis/`): Figure management
- Figure editing with provenance tracking
- Version snapshots
- Submission-ready exports

**AI** (`scitex/ai/`): AI-assisted workflows
- Unified prompt access
- Conversation history
- Domain-specific prompts (code, scholar, vis, writer)

## Usage

### Running Scripts

```bash
# Complete MNIST pipeline
make run-mnist

# Individual steps
make run-mnist-download      # Download dataset
make run-mnist-plot-digits   # Plot digit samples
make run-mnist-plot-umap     # UMAP visualization
make run-mnist-clf-svm       # Train SVM classifier
make run-mnist-conf-mat      # Confusion matrix
```

### Manuscript Writing

```bash
# Create a new manuscript project
make setup-writer
# Creates: scitex/writer/example_paper/

# Or create custom project
scitex writer clone scitex/writer/my_paper

# Git strategies available:
scitex writer clone scitex/writer/my_paper --git-strategy child   # Independent repo (default)
scitex writer clone scitex/writer/my_paper --git-strategy parent  # Track in main repo
scitex writer clone scitex/writer/my_paper --git-strategy origin  # Preserve template history
scitex writer clone scitex/writer/my_paper --git-strategy none    # No git

# Compile manuscript
cd scitex/writer/my_paper
scitex writer compile manuscript
scitex writer compile supplementary
scitex writer compile revision

# Watch mode (auto-recompile)
scitex writer watch
```

### Code Quality

```bash
# Format code
make format              # Format Python + Shell
make format-python       # Python only (ruff)
make format-shell        # Shell only (shfmt, shellcheck)

# Lint code
make lint                # Lint with ruff

# Run tests
make test                # Run all tests
make test-verbose        # Verbose output

# Complete check
make check               # format + lint + test
```

### Cleaning

```bash
make clean               # Clean outputs and logs
make clean-mnist         # Clean MNIST outputs only
make clean-data          # Clean generated data
make clean-logs          # Clean log files
make clean-python        # Clean Python cache
make clean-writer        # Remove writer projects (CAUTION!)
make clean-all           # Complete cleanup
```

### Information

```bash
make help                # Show all commands
make info                # Project information
make tree                # Directory structure
make verify              # Verify installation
make show-config         # Display configs
```

## MNIST Example Pipeline

The included MNIST example demonstrates best practices:

1. **01_download.py**: Download and preprocess MNIST data
   - Saves to `data/mnist/`
   - Creates train/test loaders
   - Generates flattened arrays

2. **02_plot_digits.py**: Visualize digit samples
   - Plots random samples
   - Shows all digit classes
   - Saves to `data/mnist/figures/`

3. **03_plot_umap_space.py**: UMAP dimensionality reduction
   - Projects to 2D space
   - Color-coded by digit class
   - Visualizes data structure

4. **04_clf_svm.py**: Train SVM classifier
   - Linear SVM on flattened images
   - Saves model to `data/mnist/models/`
   - Generates classification report

5. **05_plot_conf_mat.py**: Confusion matrix visualization
   - Analyzes classification errors
   - Heatmap visualization
   - Per-class metrics

## Development Workflow

### Adding New Scripts

1. Copy template: `cp scripts/template.py scripts/my_project/my_script.py`
2. Implement your analysis
3. Run: `cd scripts/my_project && python my_script.py`
4. Outputs automatically created in `my_script_out/`

### Adding Tests

1. Create test file: `tests/my_project/test_my_script.py`
2. Write tests using pytest
3. Run: `make test`

### Creating Manuscripts

1. Clone writer template: `scitex writer clone scitex/writer/paper_name`
2. Edit content in `00_shared/`, `01_manuscript/`, etc.
3. Compile: `scitex writer compile manuscript`
4. Commit within manuscript directory (independent git repo)

## Configuration

### PATH.yaml

Defines project-wide paths:
```yaml
BASE_DIR: /home/user/proj/scitex-research-template
DATA_DIR: ${BASE_DIR}/data
CONFIG_DIR: ${BASE_DIR}/config
SCRIPTS_DIR: ${BASE_DIR}/scripts
```

### MNIST.yaml

Project-specific settings:
```yaml
data:
  raw_dir: ${DATA_DIR}/mnist/raw
  processed_dir: ${DATA_DIR}/mnist
training:
  batch_size: 64
  epochs: 10
```

## Dependencies

Core requirements:
- Python >= 3.8
- PyTorch (for MNIST example)
- scikit-learn
- matplotlib, seaborn
- umap-learn
- scitex

Install via:
```bash
pip install -r requirements.txt
```

Development tools (optional):
```bash
make install-dev  # pytest, ruff, black, isort, mypy
```

## Git Workflow

This repository uses git for version control with special handling for writer projects:

### Main Repository
```bash
git add .
git commit -m "Your changes"
git push
```

### Writer Projects (Independent Repositories)
```bash
cd scitex/writer/my_paper
git add .
git commit -m "Update manuscript"
git push  # Pushes to manuscript's own remote
```

Or use `--git-strategy parent` to track in main repository:
```bash
scitex writer clone scitex/writer/my_paper --git-strategy parent
# Now tracked in main repo's git
```

## Testing

```bash
# Run all tests
pytest tests/

# Run specific test
pytest tests/mnist/test_01_download.py

# Verbose output
pytest tests/ -v

# With coverage
pytest tests/ --cov=scripts
```

## Troubleshooting

### Common Issues

**Import errors**: Ensure you're in the correct directory
```bash
cd scripts/mnist
python 01_download.py
```

**Missing dependencies**: Install requirements
```bash
make install
```

**Writer template clone fails**: Check scitex installation
```bash
pip install scitex
scitex --version
```

**Permission errors**: Check file permissions
```bash
chmod +x scripts/mnist/*.py
```

## Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/my-feature`
3. Make changes and test: `make check`
4. Commit: `git commit -m "Add my feature"`
5. Push: `git push origin feature/my-feature`
6. Create Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Citation

If you use this template in your research, please cite:

```bibtex
@software{scitex-research-template,
  author = {Watanabe, Yusuke},
  title = {SciTeX Template Research},
  year = {2025},
  url = {https://github.com/ywatanabe1989/scitex-research-template}
}
```

## Related Projects

- [SciTeX](https://github.com/ywatanabe1989/scitex) - Main framework
- [SciTeX Writer](https://github.com/ywatanabe1989/scitex-writer) - Manuscript template

## 📄 License

This project is licensed under the MIT License.

## 📧 Contact

Yusuke Watanabe (ywatanabe@scitex.ai)

<!-- EOF -->