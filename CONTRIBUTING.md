# Contributing to RecAI

Thank you for your interest in contributing to RecAI! This document provides guidelines and instructions for contributing.

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## How to Contribute

### Reporting Bugs

- Use the GitHub issue tracker to report bugs
- Check if the issue already exists before opening a new one
- Include a clear description of the bug and steps to reproduce
- Include relevant logs, error messages, and environment details

### Suggesting Enhancements

- Use the GitHub issue tracker for enhancement suggestions
- Provide a clear description of the enhancement
- Explain why this enhancement would be useful
- List any additional context or examples

### Pull Requests

1. Fork the repository
2. Create a new branch for your feature or bug fix (`git checkout -b feature/your-feature-name`)
3. Make your changes with clear, descriptive commit messages
4. Add or update tests if applicable
5. Update documentation as needed
6. Ensure your code follows the project's style guidelines
7. Push to your fork and submit a pull request

## Development Setup

### Prerequisites

- Python 3.9 or higher
- OpenAI API key (or Azure OpenAI API credentials)
- Git

### Environment Setup

```bash
# Clone the repository
git clone https://github.com/YOUR-USERNAME/RecAI.git
cd RecAI

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# For InteRecAgent specifically
cd InteRecAgent
pip install -r requirements.txt
```

### Configuration

1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Update `.env` with your API credentials:
   ```
   OPENAI_API_KEY=your_key_here
   OPENAI_API_TYPE=open_ai
   ```

### Running Tests

```bash
# Run tests in a specific subproject
cd InteRecAgent
pytest tests/

# Or run with unittest
python -m unittest discover tests/
```

### Code Style

- Follow PEP 8 guidelines
- Use meaningful variable and function names
- Add docstrings to functions and classes
- Keep functions focused and concise

## Commit Message Guidelines

- Use descriptive, present-tense commit messages
- Reference related issues: `Fix #123`
- Keep the first line to 50 characters or less
- Add more context in the body if needed

Example:
```
Add new ranking strategy for diversity

This commit implements a new ranking strategy that balances
relevance and diversity in recommendations. Fixes #123.
```

## Documentation

- Update README files when adding new features
- Add docstrings to new functions
- Update relevant documentation files
- Include examples for new features

## Licensing

By contributing, you agree that your contributions will be licensed under the MIT License.

## Questions?

Feel free to:
- Open an issue with the `question` label
- Check existing documentation and FAQs
- Review related papers and citations in the README

Thank you for contributing to RecAI!
