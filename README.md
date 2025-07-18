# roomodes# Roo Custom Modes Documentation

## Overview

Roo is a code extension that uses custom modes to provide specialized AI assistance for different workflow tasks. Modes are defined through JSON configuration files that specify role definitions, instructions, and permissions.

## File Structure

### Configuration Files

| File | Scope | Purpose |
|------|-------|---------|
| `custom_modes.json` | Project-specific | Modes that apply only to the current project |
| `custom_modes.yaml` | Global | Modes available across all projects |

### File Locations
- **Project modes**: Place `custom_modes.json` in your project root
- **Global modes**: Location varies by system (check roo documentation)

## JSON Schema

### Required Structure

```json
{
  "customModes": [
    {
      "slug": "mode-identifier",
      "name": "🎯 Display Name",
      "roleDefinition": "Single sentence defining what this mode IS and does",
      "customInstructions": "Detailed behavior instructions and constraints",
      "groups": ["permission1", "permission2"],
      "source": "project"
    }
  ]
}
```

### Field Definitions

#### `slug` (string, required)
- **Purpose**: Unique identifier for the mode
- **Format**: kebab-case (lowercase with hyphens)
- **Examples**: `spec-pseudocode`, `security-review`, `docs-writer`
- **Constraints**: Must be unique across all modes

#### `name` (string, required)  
- **Purpose**: Human-readable display name shown in UI
- **Format**: Emoji + descriptive title
- **Examples**: `"🧠 Auto-Coder"`, `"🛡️ Security Reviewer"`, `"📚 Documentation Writer"`
- **Best Practice**: Choose emojis that instantly communicate purpose

#### `roleDefinition` (string, required)
- **Purpose**: Concise explanation of the mode's primary responsibility
- **Format**: Single sentence defining what the mode IS and does
- **Examples**: 
  - `"You are SPARC, the orchestrator of complex workflows"`
  - `"You write clean, efficient, modular code based on pseudocode and architecture"`
  - `"You perform static and dynamic audits to ensure secure code practices"`

#### `customInstructions` (string, required)
- **Purpose**: Detailed behavioral instructions, constraints, and workflows
- **Contents**: 
  - Specific methodologies to follow
  - Validation rules and constraints
  - Delegation patterns using `new_task`
  - Completion requirements using `attempt_completion`
  - Integration points with other modes
- **Format**: Multi-line string with clear sections and bullet points

#### `groups` (array, required)
- **Purpose**: Defines what permissions/capabilities the mode has
- **Available Permissions**:
  - `"read"`: Can read files and project content
  - `"edit"`: Can modify and create files
  - `"browser"`: Can access web browsing capabilities
  - `"mcp"`: Can use Model Context Protocol features
  - `"command"`: Can execute command-line operations

#### `source` (string, required)
- **Purpose**: Indicates the scope of the mode
- **Values**: 
  - `"project"`: Project-specific mode
  - `"global"`: System-wide mode (for YAML files)

### Advanced Permission Configuration

For file-type restrictions, use object format:

```json
"groups": [
  "read",
  [
    "edit",
    {
      "fileRegex": "\\.md$",
      "description": "Markdown files only"
    }
  ]
]
```

## SPARC Methodology

### Core Principles

1. **Specification**: Clarify objectives and scope
2. **Pseudocode**: Request high-level logic with TDD anchors  
3. **Architecture**: Ensure extensible system diagrams and service boundaries
4. **Refinement**: Use TDD, debugging, security, and optimization flows
5. **Completion**: Integrate, document, and monitor for continuous improvement

### Workflow Patterns

#### Task Delegation
- Use `new_task` to assign work to other modes
- Specify clear scope and expected deliverables
- Chain modes together for complex workflows

#### Task Completion
- All modes must end tasks with `attempt_completion`
- Provide concise summary of what was accomplished
- Include next steps or recommendations

## Best Practices

### Security and Configuration
- ✅ **Never hardcode secrets** or environment variables
- ✅ **Use configuration abstraction** for environment-specific values
- ✅ **Validate input parameters** and sanitize outputs
- ✅ **Apply principle of least privilege** for permissions

### Code Organization  
- ✅ **Keep files under 500 lines** for maintainability
- ✅ **Enforce modular boundaries** between components
- ✅ **Use clear separation of concerns** across modes
- ✅ **Implement proper error handling** and logging

### Mode Design
- ✅ **Single responsibility principle** - each mode has one clear purpose
- ✅ **Clear delegation patterns** - know when to hand off to other modes
- ✅ **Consistent naming conventions** - use descriptive emojis and titles
- ✅ **Comprehensive documentation** - explain behavior and constraints

### Testing and Quality
- ✅ **Test-driven development** where applicable
- ✅ **Continuous integration** for automated validation
- ✅ **Security reviews** for all code changes
- ✅ **Performance monitoring** post-deployment

## Mode Categories

### Core SPARC Modes
- **🧠 Auto-Coder**: Implements features based on specifications
- **🧪 Tester (TDD)**: Writes tests first, implements to pass
- **🏗️ Architect**: Designs system architecture and boundaries
- **🛡️ Security Reviewer**: Audits code for security vulnerabilities
- **📚 Documentation Writer**: Creates user and technical documentation

### Orchestration Modes  
- **⚡️ SPARC Orchestrator**: Breaks down complex tasks and delegates
- **🔗 System Integrator**: Merges outputs into cohesive systems
- **❓ Ask**: Helps formulate tasks and delegate to appropriate modes

### Operations Modes
- **🚀 DevOps**: Handles deployment and infrastructure automation
- **📈 Deployment Monitor**: Observes post-launch system behavior
- **🧹 Optimizer**: Refactors and improves system performance

### Utility Modes
- **🛠️ Mode Creator**: Helps design and configure new custom modes
- **📘 SPARC Tutorial**: Onboards users to the SPARC methodology

## Integration Examples

### Simple Task Flow
```
User Request → SPARC Orchestrator → Auto-Coder → Tester → Completion
```

### Complex Project Flow  
```
User Request → SPARC Orchestrator → Specification Writer → Architect → 
Auto-Coder → Security Reviewer → Documentation Writer → System Integrator → 
DevOps → Deployment Monitor → Completion
```

### Mode Communication Pattern
```json
// Mode A delegates to Mode B
"Use `new_task` to assign work to security-review mode"

// Mode B completes and returns control  
"Finish security audit tasks with `attempt_completion`"
```

## Common Validation Rules

### File Size Limits
- Individual files must be under 500 lines
- Split large components into smaller, focused modules
- Use clear interfaces between separated components

### Environment Security
- No hardcoded API keys, passwords, or tokens
- Use environment variable abstraction layers
- Implement secure secret management practices

### Modularity Requirements  
- Clear boundaries between functional domains
- Minimal coupling between components
- Easy to test individual components in isolation

## Troubleshooting

### Common Issues
1. **Slug conflicts**: Ensure unique identifiers across all modes
2. **Permission errors**: Verify mode has appropriate groups for required actions
3. **Delegation failures**: Check that target modes exist and are accessible
4. **Completion hanging**: Ensure all modes end with `attempt_completion`

### Debugging Tips
- Check JSON syntax and structure validity
- Verify file permissions and locations
- Test mode isolation and boundaries
- Validate delegation chains and handoffs

## Contributing New Modes

1. **Identify the gap**: What workflow need isn't covered by existing modes?
2. **Define the scope**: What specific responsibility should this mode have?
3. **Design the interface**: How will it integrate with existing SPARC workflows?
4. **Create the configuration**: Use the Mode Creator or follow this documentation
5. **Test thoroughly**: Validate behavior in isolation and integration scenarios
6. **Document usage**: Update relevant documentation and examples

---

*This documentat