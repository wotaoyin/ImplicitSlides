# ImplicitSlides

Presentation slides for implicit models (deep equilibrium models). The paper proves that implicit models' expressive power scales with test-time compute, with applications in imaging, scientific computing, operations research, and LLM reasoning.

Based on the paper:

> Jialin Liu, Lisang Ding, Stanley Osher, Wotao Yin. "Implicit Models: Expressive Power Scales with Test-Time Compute". arXiv:2510.03638. [[arXiv]](https://arxiv.org/abs/2510.03638)

## Repository Structure

```
├── implicit-model.qmd      # Quarto presentation slides (Reveal.js format)
├── requirements.txt        # Python dependencies
├── overleaf/               # LaTeX paper sources
│   ├── arxiv/              # arXiv version
│   ├── figures-inverse/    # Imaging experiment figures
│   ├── figures-ns/         # Navier-Stokes experiment figures
│   ├── figures-lp/         # Linear programming experiment figures
│   ├── figures-llm/        # LLM reasoning experiment figures
│   ├── figures-toy/        # Toy example figures
│   └── tikzs/              # TikZ diagrams
└── env/                    # Python virtual environment
```

## Setup

### 1. Install uv (Python package manager)

If you don't have uv installed:

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or with Homebrew
brew install uv
```

### 2. Create Python Virtual Environment

```bash
# Create a virtual environment with Python 3.14
uv venv env --python 3.14

# Activate the virtual environment
source env/bin/activate

# Install dependencies
uv pip install -r requirements.txt
```

### 3. Install Quarto

Download and install Quarto from [quarto.org](https://quarto.org/docs/get-started/):

```bash
# macOS with Homebrew
brew install quarto

# Or download the installer from https://quarto.org/docs/get-started/
```

### 4. Install LaTeX (for PDF rendering)

If you don't have TexLive or another LaTeX distribution installed, you can use Quarto's built-in TinyTeX:

```bash
quarto install tinytex
```

This installs a minimal LaTeX distribution sufficient for rendering Quarto documents to PDF.

## Usage

### Quarto Presentations

```bash
# Preview with live reload (opens in browser)
quarto preview implicit-model.qmd

# Render presentation to HTML
quarto render implicit-model.qmd
```

## Key Conventions

- Quarto presentations use `format: revealjs` with BibTeX citations via `bibliography: overleaf/arxiv/refs.bib`
- LaTeX papers import `math_commands.tex` for consistent notation:
  - Vectors: `\vx`, `\vy`
  - Operators: `\cF`, `\cG`
  - Matrices: `\mI`
