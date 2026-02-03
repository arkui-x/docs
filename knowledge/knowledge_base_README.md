# ArkUI-X Knowledge Base

This directory contains comprehensive knowledge base documentation for in-depth component analysis and development guidance.

## Knowledge Base Structure

### 1. Architecture (`architecture/`)

System architecture design and component documentation.

| Document | Description |
|----------|-------------|
| `system_architecture.md` | Complete layered architecture, directory mapping, technical features, component lifecycle, cross-platform adaptation, build system architecture |

**Topics Covered:**
- 5-Layer Architecture Design
- Directory Structure Mapping
- Core Technical Features
- Component Lifecycle
- Cross-Platform Adaptation Architecture
- Build System Architecture
- Architecture Principles

### 2. Plugin Adaptation (`plugin_adaptation/`)

Platform adaptation best practices for subsystem APIs.

| Document | Description |
|----------|-------------|
| `adapter_guide.md` | Platform adaptation rules, implementation steps, build system configuration, reference implementation |

**Topics Covered:**
- 13 Core Rules
- Adaptation Checklist
- Reference Implementation (plugins/net/http)
- Implementation Steps
- Build System Configuration (4 required files)
- Common Issues and Solutions
- Applicable Modules List

### 3. Best Practices (`best_practices/`)

Development guidelines and optimization strategies.

| Document | Description |
|----------|-------------|
| `development_guide.md` | Environment setup, build system, application development, framework development, testing & debugging, performance optimization |

**Topics Covered:**
- Environment Configuration (Linux/macOS)
- Build System Best Practices
- Application Development Best Practices
- Framework Development Best Practices
- Testing and Debugging Guide
- Performance Optimization Suggestions
- Common Issues and Solutions

## Usage Guidelines

When answering questions or providing guidance:

1. **Check for relevant knowledge base documents** before diving into code analysis
2. **Search the knowledge base** using Grep tools to find component-specific information
3. **Reference knowledge base content** to provide comprehensive, context-aware answers
4. **Cross-reference with actual code** using the file paths and line numbers cited in knowledge base documents

## Quick Reference Table

| Topic | Document Location |
|-------|-------------------|
| System Architecture | `architecture/system_architecture.md` |
| Plugin Platform Adaptation | `plugin_adaptation/adapter_guide.md` |
| Development Best Practices | `best_practices/development_guide.md` |

## Document Organization Principles

### File Naming Convention

- **Use descriptive names** that reflect content (not generic `README.md`)
- **Group related content** in single documents
- **Split unrelated content** into separate documents

### Examples

```
# Good naming
architecture/system_architecture.md
architecture/component_lifecycle.md
best_practices/environment_setup.md
best_practices/build_system.md
plugin_adaptation/http_adapter_guide.md

# Avoid
architecture/README.md
best_practices/README.md
```

## Maintenance Guidelines

### When Updating Documentation

1. **Verify actual code first**
   - Use Read/Grep to confirm current implementation
   - Compare documented behavior with actual code

2. **Update documentation**
   - Fix incorrect information
   - Update code references (file paths, line numbers)
   - Ensure all examples match actual code

3. **Cross-reference with CLAUDE.md**
   - Keep CLAUDE.md as high-level summary
   - Link to detailed knowledge base documents
   - Maintain consistency between documents

---

**Note**: This knowledge base is continuously evolving. When discovering new patterns or best practices, document them in the appropriate category.
