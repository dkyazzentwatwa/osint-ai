# Contributing to OSINT Investigator

Thank you for your interest in contributing! This document provides guidelines for contributing to the OSINT Investigator skill.

## How to Contribute

### Reporting Issues

1. Check if the issue already exists
2. Provide clear reproduction steps
3. Include example inputs/outputs
4. Note your environment (agent type, version)

### Suggesting Features

1. Open a discussion first for major changes
2. Explain the use case
3. Describe expected behavior

### Adding Dork Patterns

Dork patterns are in `references/dork-library.md`:

```markdown
### Platform Name
- `site:platform.com "target"` - Description of what this finds
- `site:platform.com inurl:profile "target"` - Another pattern
```

Requirements:
- Test the dork actually works
- Include a brief description
- Categorize by target type (person, domain, org)

### Adding Reconnaissance Vectors

Vectors are in `references/recon-vectors.md`:

Follow the existing structure:
1. Target type identification
2. Search sequence
3. Expected findings
4. Pivot opportunities

### Improving Documentation

- Keep language clear and concise
- Include examples
- Update table of contents if adding sections

## Code Style

### For SKILL.md updates

- Use consistent heading levels
- Include confidence rating examples
- Add source citations where relevant

### For Report Templates

- Maintain professional tone
- Include all required sections
- Ensure markdown renders correctly

## Pull Request Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### PR Requirements

- Clear description of changes
- Reference any related issues
- Test that skill still loads correctly
- Update documentation if needed

## Development Setup

To test changes:

1. Copy modified files to your agent's skill directory
2. Restart or reload your agent
3. Test commands in a fresh conversation
4. Verify no syntax errors in markdown

## Areas Needing Help

- [ ] More platform-specific dork patterns
- [ ] Additional reconnaissance vectors for:
  - Cryptocurrency addresses
  - Vehicle registration lookups
  - Property records
- [ ] Translation to other languages
- [ ] Video tutorial scripts
- [ ] Example investigation walkthroughs

## Questions?

- Open a Discussion for general questions
- Comment on existing Issues/PRs
- Tag maintainers for urgent issues

## Code of Conduct

- Be respectful and constructive
- Focus on the skill, not the person
- Welcome newcomers
- Assume good intent

Thank you for contributing to open-source OSINT!
