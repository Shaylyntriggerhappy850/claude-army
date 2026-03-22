# Context Guide

The `.context/` folder is how you feed information to the forge. The more relevant context you provide, the better the generated agents will be.

## Structure

```
.context/
├── task.md              # Required: what you want to accomplish
├── constraints.md       # Optional: rules, requirements, limitations
├── research/            # Optional: background docs, specs, references
│   ├── api-docs.md
│   └── competitor-analysis.md
└── examples/            # Optional: examples of what good output looks like
    └── similar-work/
```

## task.md

Describe what you want to accomplish. Be specific:

```markdown
# Task

Contribute a new tax regime for [country] to the GOBL project.

## What needs to happen
- Research the country's tax system
- Implement tax identity validation
- Create invoice validation rules
- Write tests with 90%+ coverage
- Submit a PR

## Quality bar
- Must follow the project's existing patterns
- Must pass the lead reviewer's review
```

## constraints.md

Any hard rules the agents must follow:

```markdown
# Constraints

- Language: Go 1.24+
- No external dependencies beyond what's already in go.mod
- Must be backwards compatible
- PR must be opened as draft first
```

## research/

Any background information that saves the agents from searching:
- API documentation
- Specifications
- Previous discussions or decisions
- Related work in other projects

## examples/

Examples of what the output should look like. The forge reads these to understand quality expectations. For instance, an existing implementation that the new work should mirror.
