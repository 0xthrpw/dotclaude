# CLAUDE.md - Development Framework Directive

## Overview
This document directs Claude Code to follow a structured four-phase development framework: **Assess → Plan → Execute → Test**. Always reference this framework when working on development tasks and maintain organized documentation throughout the process.

## Development Framework Protocol

### Phase 1: ASSESS
**Before starting any development work, always begin with assessment:**

1. **Project Context Analysis**
   - Review existing codebase structure and architecture
   - Identify current project state and technical debt
   - Understand dependencies and technology stack
   - Ask clarifying questions about requirements and constraints

2. **Requirement Clarification**
   - Define functional and non-functional requirements explicitly
   - Identify expected inputs, outputs, and success criteria
   - Document assumptions and edge cases
   - Confirm technical specifications and performance requirements

3. **Resource Assessment**
   - Evaluate complexity and time estimates
   - Identify knowledge gaps or research needs
   - Assess testing requirements and environments
   - Determine integration points with existing systems

**Create or update: `assess.md` with findings and requirements**

### Phase 2: PLAN
**Always create a structured implementation plan before coding:**

1. **Task Decomposition**
   - Break down work into smaller, manageable components
   - Identify task dependencies and optimal execution order
   - Define clear deliverables for each component
   - Prioritize based on risk and business value

2. **Architecture Design**
   - Design overall system structure and interfaces
   - Define data flows and component interactions
   - Plan for scalability, maintainability, and extensibility
   - Choose appropriate design patterns and frameworks

3. **Implementation Strategy**
   - Define coding standards and conventions
   - Plan error handling and logging strategies
   - Identify reusable components and libraries
   - Create development and testing approach

**Create or update: `plan.md` with detailed implementation roadmap**

### Phase 3: EXECUTE
**Follow planned approach while maintaining quality:**

1. **Code Generation Guidelines**
   - Implement according to the established plan
   - Follow defined coding standards and conventions
   - Include comprehensive comments and documentation
   - Generate code in small, testable increments

2. **Quality Assurance**
   - Review generated code for correctness and efficiency
   - Ensure adherence to architectural decisions
   - Validate integration with existing codebase
   - Maintain consistent style and patterns

3. **Documentation Creation**
   - Generate inline comments and docstrings
   - Update README files and API documentation
   - Document configuration and setup procedures
   - Create usage examples and tutorials

**Create or update: `execute.md` with implementation progress and decisions**

### Phase 4: TEST
**Validate all implementations thoroughly:**

1. **Test Planning**
   - Define test strategies and coverage requirements
   - Identify test cases, scenarios, and edge cases
   - Plan unit, integration, and system testing
   - Create test data and mock requirements

2. **Test Implementation**
   - Generate comprehensive unit tests
   - Create integration tests for system interactions
   - Implement automated testing pipelines
   - Include performance and security testing

3. **Issue Resolution**
   - Debug failing tests and identify root causes
   - Refactor code to improve testability
   - Validate fixes with regression testing
   - Document known issues and workarounds

**Create or update: `test.md` with test results and coverage reports**

## File Organization Standards

### Required Project Files
- `assess.md` - Requirements, constraints, and project analysis
- `plan.md` - Implementation roadmap and architectural decisions
- `execute.md` - Development progress and implementation notes
- `test.md` - Test plans, results, and coverage reports
- `README.md` - Project overview and setup instructions
- `CHANGELOG.md` - Version history and changes

### Documentation Guidelines
- Use clear, descriptive headers and consistent formatting
- Include timestamps for major updates
- Link related documents and maintain cross-references
- Use code blocks for examples and configurations
- Include decision rationale and context

## Workflow Instructions

### Starting a New Development Session
1. Always begin by reviewing existing project documentation
2. Update `assess.md` with current session objectives
3. Reference `plan.md` for planned approach
4. Follow established architecture and coding standards
5. Document decisions and progress in real-time

### During Development
- Make incremental commits with clear messages
- Update relevant documentation files continuously
- Ask for clarification when requirements are ambiguous
- Validate implementations against planned architecture
- Maintain test coverage throughout development

### Completing Development Work
- Ensure all phases are properly documented
- Run comprehensive tests and update `test.md`
- Update `README.md` with new features or changes
- Create or update `CHANGELOG.md` entries
- Perform final code review and cleanup

## Code Quality Standards

### General Guidelines
- Follow established project coding conventions
- Write self-documenting code with clear variable names
- Include comprehensive error handling
- Implement proper logging and monitoring
- Design for testability and maintainability

### Documentation Requirements
- Every function/method must have docstrings
- Complex algorithms require inline comments
- API endpoints need comprehensive documentation
- Configuration options must be clearly documented
- Include usage examples for public interfaces

### Testing Requirements
- Minimum 80% code coverage for new code
- Unit tests for all public methods
- Integration tests for external dependencies
- Performance tests for critical paths
- Security tests for authentication/authorization

## Communication Protocol

### Status Updates
- Provide regular progress updates during long tasks
- Explain complex decisions and trade-offs
- Alert to potential issues or blockers early
- Request feedback on architectural decisions
- Confirm understanding of requirements

### Error Handling
- Clearly explain any errors or failures
- Provide specific steps to reproduce issues
- Suggest alternative approaches when blocked
- Document workarounds and temporary solutions
- Escalate complex technical decisions

### Code Review Process
- Explain the purpose and approach of generated code
- Highlight any assumptions or limitations
- Provide testing recommendations
- Suggest improvements or optimizations
- Document any technical debt incurred

## Artifact Management

### When to Create Artifacts
- Complete modules, classes, or substantial functions
- Comprehensive documentation and guides
- Configuration files and templates
- Test suites and test data
- Architectural diagrams and specifications

### Artifact Naming Conventions
- Use descriptive, consistent naming
- Include version information where appropriate
- Maintain clear relationships between artifacts
- Document artifact purposes and usage
- Keep artifacts updated with code changes

## Continuous Improvement

### Regular Reviews
- Conduct retrospectives after major milestones
- Identify process improvements and bottlenecks
- Update framework based on lessons learned
- Share knowledge and best practices
- Maintain and refine documentation templates

### Metrics and Success Criteria
- Track development velocity and quality metrics
- Monitor test coverage and defect rates
- Measure documentation completeness
- Assess adherence to framework phases
- Evaluate overall project success

---

## Framework Adherence Checklist

Before considering any development task complete, verify:
- [ ] Assessment phase completed with documented requirements
- [ ] Implementation plan created and followed
- [ ] Code generated according to standards and architecture
- [ ] Comprehensive tests written and passing
- [ ] All documentation updated and accurate
- [ ] Progress tracked in appropriate markdown files
- [ ] Code reviewed for quality and maintainability
- [ ] Integration validated with existing systems

**Remember: This framework is designed to improve development quality and velocity. Always follow these phases in order and maintain comprehensive documentation throughout the process.**