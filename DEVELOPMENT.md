# Development Guide for RecAI Contributors

This guide provides detailed information for developers contributing to RecAI.

## Project Structure

```
RecAI/
├── .github/
│   └── workflows/                 # GitHub Actions CI/CD workflows
├── InteRecAgent/                  # Interactive Recommender Agent
│   ├── llm4crs/                  # Core agent logic
│   │   ├── agent.py              # Main agent class
│   │   ├── ranking/              # Ranking tools
│   │   ├── retrieval/            # Retrieval tools
│   │   ├── query/                # Query modules
│   │   ├── memory/               # Conversation memory
│   │   ├── prompt/               # Prompt templates
│   │   └── utils/                # Utilities
│   ├── tests/                     # Unit tests
│   ├── demonstration/             # Demo examples
│   ├── app.py                    # Gradio web interface
│   └── requirements.txt           # Python dependencies
├── Knowledge_Plugin/              # Knowledge injection module
├── RecExplainer/                  # Model explanation module
├── RecLM-emb/                     # Embedding models
├── RecLM-eval/                    # Evaluation framework
├── RecLM-gen/                     # Generative models
├── RecLM-uni/                     # Unified LLM recommender
├── .env.example                  # Environment template
├── .gitignore                    # Git ignore rules
├── CONTRIBUTING.md               # Contribution guidelines
├── DEVELOPMENT.md                # This file
├── SETUP.md                      # Installation guide
└── README.md                     # Project overview
```

## Development Workflow

### 1. Setting Up Development Environment

```bash
# Clone repository
git clone https://github.com/YOUR-USERNAME/RecAI.git
cd RecAI

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
# or
.\venv\Scripts\activate.ps1  # Windows PowerShell

# Install development dependencies
pip install -r requirements-dev.txt  # if available
# or install main requirements
pip install -e ".[dev]"  # if setup.py is configured
```

### 2. Making Changes

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make changes in the appropriate subproject**

3. **Test your changes** (see Testing section)

4. **Commit with clear messages:**
   ```bash
   git add .
   git commit -m "Add feature description (fixes #123)"
   ```

5. **Push and create PR:**
   ```bash
   git push origin feature/your-feature-name
   ```

## Code Style Guidelines

### Python Style

We follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) with some modifications:

- **Line length:** 120 characters (not 80)
- **Imports:** Use `isort` for import ordering
- **Formatting:** Use `black` for consistent formatting
- **Type hints:** Use type hints for function signatures

### Example Function

```python
from typing import Dict, List, Optional
from loguru import logger


def process_recommendations(
    items: List[Dict[str, str]],
    max_count: int = 10,
    filter_criteria: Optional[Dict] = None,
) -> List[Dict[str, str]]:
    """
    Process and filter recommendations.
    
    Args:
        items: List of item dictionaries
        max_count: Maximum number of recommendations to return
        filter_criteria: Optional filtering criteria
    
    Returns:
        Filtered list of recommendations
        
    Raises:
        ValueError: If items is empty
    """
    if not items:
        raise ValueError("Items list cannot be empty")
    
    logger.info(f"Processing {len(items)} recommendations")
    
    # Implementation here
    return items[:max_count]
```

### Docstring Format

Use Google-style docstrings:

```python
def function_name(param1: str, param2: int = 0) -> bool:
    """
    Brief description of what the function does.
    
    More detailed explanation if needed.
    
    Args:
        param1: Description of param1
        param2: Description of param2, defaults to 0
    
    Returns:
        Description of return value
        
    Raises:
        SomeError: When this error occurs
        
    Example:
        >>> function_name("example", 5)
        True
    """
    pass
```

## Testing

### Running Tests

```bash
# Run all tests
pytest tests/ -v

# Run specific test file
pytest tests/test_agent.py -v

# Run with coverage
pytest tests/ --cov=llm4crs --cov-report=html

# Run in a specific subproject
cd InteRecAgent
pytest tests/ -v
```

### Writing Tests

Create test files following this pattern: `test_*.py` or `*_test.py`

```python
import pytest
from llm4crs.agent import CRSAgent


class TestCRSAgent:
    """Test suite for CRSAgent class."""
    
    @pytest.fixture
    def agent(self):
        """Fixture to create agent instance."""
        # Setup code here
        yield agent
        # Teardown code here
    
    def test_agent_initialization(self, agent):
        """Test agent initializes correctly."""
        assert agent is not None
        assert agent.mode == 'accuracy'
    
    def test_agent_set_mode(self, agent):
        """Test setting agent mode."""
        agent.set_mode('diversity')
        assert agent.mode == 'diversity'
    
    def test_invalid_mode_raises_error(self, agent):
        """Test invalid mode raises AssertionError."""
        with pytest.raises(AssertionError):
            agent.set_mode('invalid')
```

## Debugging

### Using Logging

RecAI uses `loguru` for logging:

```python
from loguru import logger

# Simple logging
logger.info("Processing started")
logger.debug(f"Item count: {len(items)}")
logger.warning("Fallback strategy used")
logger.error("Failed to retrieve items")

# Structured logging
logger.debug("User query", query=user_input, domain=domain)
```

### Setting Log Level

```bash
# Via environment variable
export LOGURU_LEVEL=DEBUG

# In Python
import logging
from loguru import logger
logger.enable("llm4crs")
logger.addlevel(logging.DEBUG)
```

### Debugging with IDE

**VS Code (launch.json):**
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python App",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/app.py",
            "console": "integratedTerminal",
            "args": ["--engine", "gpt-4"],
            "env": {
                "DOMAIN": "game",
                "OPENAI_API_KEY": "${env:OPENAI_API_KEY}"
            }
        }
    ]
}
```

## Common Development Tasks

### Adding a New Tool

1. Create a new file in `llm4crs/ranking/` or `llm4crs/retrieval/`
2. Implement the tool class with `name`, `desc`, and `run` method
3. Register the tool in `app.py`
4. Write tests in `tests/test_your_tool.py`
5. Update documentation

Example:

```python
class CustomTool:
    def __init__(self):
        self.name = "custom_tool"
        self.desc = "Description of what this tool does"
    
    def run(self, input_str: str) -> str:
        # Implementation
        return result
```

### Adding a New Feature

1. Create a new branch
2. Implement the feature
3. Write tests
4. Update relevant documentation
5. Submit PR with clear description

### Updating Dependencies

```bash
# Update all packages
pip install --upgrade -r requirements.txt

# Update specific package safely
pip install --upgrade package_name

# Save updated requirements
pip freeze > requirements.txt
```

## Performance Optimization

### Profiling

```python
import cProfile
import pstats

profiler = cProfile.Profile()
profiler.enable()

# Your code here

profiler.disable()
stats = pstats.Stats(profiler)
stats.sort_stats('cumulative')
stats.print_stats(20)  # Show top 20 functions
```

### Memory Profiling

```bash
pip install memory-profiler
python -m memory_profiler script.py
```

## Documentation

### Building Documentation

```bash
# If using Sphinx
cd docs
make html
open _build/html/index.html
```

### Updating README

- Keep README.md at project root updated
- Update subproject READMEs when making changes
- Include examples for new features
- Add links to relevant papers

## Before Submitting PR

- [ ] Code follows PEP 8 style guidelines
- [ ] All tests pass: `pytest tests/ -v`
- [ ] No print statements (use logging instead)
- [ ] No hardcoded API keys or credentials
- [ ] Updated relevant documentation
- [ ] Commit messages are clear and descriptive
- [ ] No merge conflicts
- [ ] Reviewed your own code first

## Useful Resources

- [Python PEP 8](https://www.python.org/dev/peps/pep-0008/)
- [Python Type Hints](https://docs.python.org/3/library/typing.html)
- [Pytest Documentation](https://docs.pytest.org/)
- [Git Workflow](https://guides.github.com/introduction/flow/)
- [LangChain Documentation](https://python.langchain.com/)
- [OpenAI API Documentation](https://platform.openai.com/docs/)

## Getting Help

- Check existing issues and PRs
- Ask questions in issue discussions
- Review project papers and documentation
- Check CI/CD workflow outputs for errors

Happy coding! 🚀
