Convert Python classes to Pydantic models throughout this codebase.

Requirements:
- Replace hand-crafted classes with Pydantic BaseModel where appropriate
- Preserve all existing functionality and behavior
- Keep type hints (add them if missing)
- Use Pydantic features: Field(), default values, validators, frozen models, etc.
- Convert __init__, __repr__, __eq__ to Pydantic equivalents
- Keep custom methods that aren't replaced by Pydantic features
- Leverage Pydantic's automatic validation and serialization

Do NOT convert if:
- Class has complex __init__ logic that conflicts with Pydantic's validation
- Class uses __slots__ for performance
- Class inherits from non-Pydantic base classes (unless trivial)
- Class implements custom __new__, __setattr__, or other special methods that conflict
- Runtime validation overhead would cause performance issues in hot paths

For each conversion:
1. Add `from pydantic import BaseModel, Field` (and ConfigDict, validator, etc. if needed)
2. Replace class definition to inherit from BaseModel
3. Remove redundant __init__, __repr__, __eq__ methods
4. Use Field() for validation, defaults, and metadata
5. Add validators with @field_validator or @model_validator if needed
6. Use model_config for frozen models or other configurations
7. Run existing tests to verify behavior unchanged

Start with simple classes. Ask before converting complex cases.
