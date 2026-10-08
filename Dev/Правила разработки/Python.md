1. Используем uv с pyproject.toml
2. Линтеры:
	1. ruff
	2. mypy

Пример pyproject.tom:

	```toml
	[project]  
	name = "your-project"  
	version = "0.1.0"  
	description = "Your project description"  
	requires-python = ">=3.11"  
	dependencies = []  
	
	[tool.uv]  
	dev-dependencies = [  
		"mypy>=1.13.0",  
		"ruff>=0.8.0",  
		"pre-commit>=3.0.0",  
		"pytest>=8.0.0",  
	]  
	[tool.ruff]  
	target-version = "py311"  
	line-length = 100  
	[tool.ruff.lint]  
	ignore = [  
		"E501", # line too long (handled by formatter)  
	]  
	[tool.ruff.lint.isort]  
	known-first-party = ["your_project"]  
	[tool.mypy]  
	python_version = "3.11"  
	strict = true  
	warn_return_any = true  
	warn_unused_configs = true  
	disallow_untyped_defs = true  
	disallow_any_generics = true  
	check_untyped_defs = true  
	no_implicit_reexport = true  
	warn_redundant_casts = true  
	warn_unused_ignores = true
	```
