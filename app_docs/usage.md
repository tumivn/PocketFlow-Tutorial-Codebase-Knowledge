# Usage Guide for Codebase Knowledge Builder

## 1. Introduction

This document provides instructions on how to install and use the Codebase Knowledge Builder application, which analyzes codebases and generates comprehensive documentation in the form of tutorials or Software Architecture Documents (SAD).

## 2. Installation

### 2.1 Prerequisites

- Python 3.x
- Git (for cloning repositories)
- Access to an LLM service (configured through environment variables)

### 2.2 Installation Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/The-Pocket/Tutorial-Codebase-Knowledge.git
   cd Tutorial-Codebase-Knowledge
   ```

2. Install the required packages:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up environment variables:
   Create a `.env` file in the root directory with the following variables:
   ```
   GITHUB_TOKEN=your_github_token  # Optional, for accessing private repositories
   OPENAI_API_KEY=your_openai_api_key  # If using OpenAI
   # Other LLM service API keys as needed
   ```

## 3. Command-Line Arguments

The application supports the following command-line arguments:

```
python main.py [--repo REPO | --dir DIR] [options]
```

### 3.1 Required Arguments (Mutually Exclusive)

- `--repo REPO`: URL of the public GitHub repository to analyze.
- `--dir DIR`: Path to local directory to analyze.

### 3.2 Optional Arguments

- `-n, --name NAME`: Project name (optional, derived from repo/directory if omitted).
- `-t, --token TOKEN`: GitHub personal access token (optional, reads from GITHUB_TOKEN env var if not provided).
- `-o, --output OUTPUT`: Base directory for output (default: ./output).
- `-i, --include INCLUDE [INCLUDE ...]`: Include file patterns (e.g., '*.py' '*.js'). Defaults to common code files if not specified.
- `-e, --exclude EXCLUDE [EXCLUDE ...]`: Exclude file patterns (e.g., 'tests/*' 'docs/*'). Defaults to test/build directories if not specified.
- `-s, --max-size MAX_SIZE`: Maximum file size in bytes (default: 100000, about 100KB).
- `--language LANGUAGE`: Language for the generated documentation (default: english).
- `--doc-type {tutorial,sad}`: Type of document to generate: tutorial or Software Architecture Document (SAD) (default: tutorial).

## 4. Usage Examples

### 4.1 Generate a Tutorial for a GitHub Repository

```bash
python main.py --repo https://github.com/username/repo
```

This will generate a tutorial for the specified GitHub repository using the default settings.

### 4.2 Generate a Tutorial for a Local Directory

```bash
python main.py --dir /path/to/local/directory
```

This will generate a tutorial for the specified local directory using the default settings.

### 4.3 Generate a Software Architecture Document (SAD)

```bash
python main.py --repo https://github.com/username/repo --doc-type sad
```

This will generate a Software Architecture Document (SAD) for the specified GitHub repository.

### 4.4 Generate Documentation in a Different Language

```bash
python main.py --repo https://github.com/username/repo --language spanish
```

This will generate a tutorial for the specified GitHub repository in Spanish.

### 4.5 Customize File Filtering

```bash
python main.py --repo https://github.com/username/repo --include "*.py" "*.js" --exclude "tests/*" "docs/*"
```

This will generate a tutorial for the specified GitHub repository, including only Python and JavaScript files, and excluding files in the tests and docs directories.

### 4.6 Specify Output Directory

```bash
python main.py --repo https://github.com/username/repo --output my_output
```

This will generate a tutorial for the specified GitHub repository and save the output to the my_output directory.

## 5. Output Structure

The generated documentation is saved to the specified output directory (default: ./output). The output directory contains:

- `index.md`: The main index file that provides an overview and links to all chapters.
- Chapter files: One file per chapter, named according to the chapter number and title.

For example:
```
output/
├── project_name/
│   ├── index.md
│   ├── 01_chapter_title.md
│   ├── 02_chapter_title.md
│   └── ...
```

## 6. Best Practices

### 6.1 Repository Selection

- Choose repositories with clear structure and organization for best results.
- Smaller repositories (up to a few hundred files) work best due to LLM context limitations.
- Repositories with good documentation and comments will yield better results.

### 6.2 File Filtering

- Use include/exclude patterns to focus on the most relevant files.
- Exclude test files, build artifacts, and other non-essential files to improve analysis quality.
- Limit the maximum file size to avoid processing very large files that might not be essential for understanding the codebase.

### 6.3 Language Selection

- Choose the language that best suits your target audience.
- Be aware that translation quality may vary depending on the technical complexity of the codebase and the chosen language.

### 6.4 Document Type Selection

- Use the tutorial mode for beginner-friendly explanations of the codebase.
- Use the SAD mode for more formal architectural documentation following standard practices.

## 7. Troubleshooting

### 7.1 Common Issues

- **Rate Limiting**: If you encounter rate limiting issues when accessing GitHub repositories, provide a GitHub token using the `--token` option or the `GITHUB_TOKEN` environment variable.
- **LLM API Errors**: Ensure that you have set up the correct environment variables for your LLM service.
- **Memory Issues**: If you encounter memory issues when processing large repositories, try filtering files more aggressively using include/exclude patterns.

### 7.2 Error Messages

- **"No GitHub token provided"**: This is a warning that you might hit rate limits when accessing public repositories. You can ignore it for small repositories or provide a token for larger ones.
- **"Error fetching repository"**: Check that the repository URL is correct and accessible, and that you have provided a valid GitHub token if the repository is private.
- **"Error crawling directory"**: Check that the directory path is correct and accessible.

## 8. Support

If you encounter any issues or have questions about using the Codebase Knowledge Builder, please open an issue on the GitHub repository:

https://github.com/The-Pocket/Tutorial-Codebase-Knowledge/issues