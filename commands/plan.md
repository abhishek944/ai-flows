# PLAN MODE - Comprehensive Feature Planning System

## Overview
This is a specialized planning mode for AI coding assistants that ensures thorough analysis, exhaustive clarification, and detailed documentation before any implementation begins. The system operates in read-only mode for the codebase (<ADD_YOUR_REPO>-* folders) to preserve codebase integrity, but can write and update planning artifacts in the ai-flows/ folder.

## Core Principles

### 1. Deep File Analysis
- **Mandatory**: Read and understand EVERY file in the codebase that relates to the feature
- **Depth**: Analyze not just the structure but also the patterns, dependencies, conventions, and architectural decisions
- **Context**: Understand how files interact with each other, data flows, and system boundaries
- **Documentation**: Review existing documentation, comments, and code patterns to understand the project's philosophy

### 2. Strict Read-Only Mode for Codebase
- **READ-ONLY FOR <ADD_YOUR_REPO>-* FOLDERS**: Under no circumstances should any file in `<ADD_YOUR_REPO>-backend/`, `<ADD_YOUR_REPO>-frontend/`, or `<ADD_YOUR_REPO>-admin/` be modified during planning phase
- **NO CODEBASE FILE CREATION**: Do not create any files in <ADD_YOUR_REPO>-* folders
- **ONLY READING FROM CODEBASE**: Only read files from <ADD_YOUR_REPO>-* folders to gather information and understand the codebase
- **PRESERVE CODEBASE INTEGRITY**: Maintain the current state of all files in <ADD_YOUR_REPO>-* folders without any changes

### 2a. Write Access for Planning Artifacts
- **WRITE ALLOWED IN AI-FLOWS/**: Planning artifacts can be written and updated in `ai-flows/<feature_name>/` folder
- **ALLOWED OPERATIONS**: Create, read, and update files in ai-flows/ folder including:
  - `plan.md` - The detailed implementation plan
  - `plan-questions.md` - Questions and answers
  - `review-plan.md` - Review comments and feedback
  - `todo.md` - Implementation todos
- **DIRECTORY CREATION**: Can create `ai-flows/<feature_name>/` directory structure as needed

### 3. Exhaustive Questioning Protocol
- **Before Planning**: Ask comprehensive questions to clarify every aspect
- **No Assumptions**: Do not assume anything - ask about:
  - Feature requirements and scope
  - User experience expectations
  - Technical constraints and preferences
  - Integration points and dependencies
  - Edge cases and error handling
  - Performance considerations
  - Security implications
- **Multiple Choice Format**: Questions should include multiple choice options when applicable to make it easier for users to select answers
- **User Answers Only**: The agent MUST NOT fill in answers. Only the user should provide answers in the "User Reply" section
- **Question Limits**: Cap the total number of active questions per feature complexity
  - Easy features: maximum 5 questions
  - Medium features: maximum 15 questions
  - Big features: maximum 25 questions
- **Iterative**: Continue asking questions until all ambiguities are resolved
- **Document Everything**: All questions must be recorded in the designated questions file with "User Reply: __" placeholders

### 4. Feature Naming Convention
- **Format**: `FEATURE: <feature_name>` at the top of the plan
- **Rules**: 
  - No spaces allowed
  - No special characters allowed (except underscores or hyphens if necessary)
  - Use lowercase or camelCase
  - Keep it descriptive but concise
  - Examples: `userAuthentication`, `payment-processing`, `data-export`

### 5. File Organization Structure
All planning artifacts must be organized as follows:

```
ai-flows/
  <feature_name>/
    plan.md              # The detailed implementation plan (with phases if needed)
    plan-questions.md    # All questions asked and answers received
    review-plan.md       # Review comments and feedback
    todo.md              # Implementation todos extracted from the plan
```

**Important**: 
- All planning files are written to `ai-flows/<feature_name>/` folder
- Never modify files in `<ADD_YOUR_REPO>-backend/`, `<ADD_YOUR_REPO>-ui/`, or `<ADD_YOUR_REPO>-admin/` folders during planning
- Only read from <ADD_YOUR_REPO>-* folders to understand the codebase structure and patterns

## Content Constraints

- **No Code Snippets**: `plan.md` and `todo.md` must never include code snippets, pseudo-code, or literal API payloads—describe intent and behavior only.
- **Plan Length Caps**: Keep the total line count of `plan.md` within the feature complexity limits:
  - Easy features: maximum 100 lines
  - Medium features: maximum 200 lines
  - Big features: maximum 300 lines
- **Question Limits**: Stay within the complexity-based question caps defined in the Exhaustive Questioning Protocol; consolidate or defer lower-priority questions before exceeding the limit.
- **Scope Exclusions**: Do not cover testing, deployment, documentation updates, or future enhancements anywhere in the plan, questions, or todos.

## Invocation Modes

This plan command can be invoked in three scenarios. You MUST determine which scenario applies:

### Scenario A: New Plan Creation
**Indicators:**
- No existing `ai-flows/<feature_name>/` directory
- User explicitly requests a new plan
- No existing plan.md file found

**Workflow:**
1. **Discover Feature Name**: 
   - If user provides feature name, use it (validate naming rules)
   - If not provided, ask user for feature name first
   - Validate feature name follows naming conventions

2. **Initial Discovery**:
   - **READ ONLY FROM <ADD_YOUR_REPO>-* FOLDERS**: Search for and read all relevant files in `<ADD_YOUR_REPO>-backend/`, `<ADD_YOUR_REPO>-ui/`, and `<ADD_YOUR_REPO>-admin/`
   - Understand the project structure, tech stack, and patterns
   - Identify areas that will be affected by the feature
   - **DO NOT MODIFY**: Only read files, never edit or create files in <ADD_YOUR_REPO>-* folders

3. **Question Generation**:
   - Generate comprehensive questions covering all aspects
   - Include multiple choice options when applicable
   - Add "User Reply: __" section after each question
   - Stay within the complexity-based caps (5/15/25 questions); consolidate before exceeding the limit
   - Do not ask about testing, deployment, documentation updates, or future enhancements
   - **WRITE TO AI-FLOWS/**: Save questions to `ai-flows/<feature_name>/plan-questions.md`
   - Present questions to user and wait for them to fill in "User Reply" sections

4. **Plan Creation** (after user fills in answers):
   - Read user-provided answers from "User Reply" sections in `plan-questions.md`
   - **WRITE TO AI-FLOWS/**: Create directory: `ai-flows/<feature_name>/` (if not already created)
   - Assess feature complexity and determine if phase breakdown is needed
   - **WRITE TO AI-FLOWS/**: Create detailed plan in `ai-flows/<feature_name>/plan.md` based on user answers
   - Include FEATURE header at the top
   - Keep plan text within the appropriate line cap (100/200/300) and never include code snippets
   - Focus strictly on implementation scope—exclude testing, deployment, documentation updates, and future enhancements
   - If feature is large/complex, break into multiple phases with detailed plan for each
   - Ensure plan is exhaustive and actionable
   - **WRITE TO AI-FLOWS/**: Extract implementation todos to `ai-flows/<feature_name>/todo.md`

### Scenario B: Questions Answered
**Indicators:**
- `ai-flows/<feature_name>/plan-questions.md` exists with unanswered questions
- User has filled in "User Reply" sections in the questions file
- `plan.md` may or may not exist yet

**Workflow:**
1. **Read Context**:
   - **READ FROM AI-FLOWS/**: Read existing `ai-flows/<feature_name>/plan-questions.md` to see previous questions
   - Identify which questions have "User Reply" sections filled in by the user
   - **READ FROM AI-FLOWS/**: Read any existing `ai-flows/<feature_name>/plan.md` if it exists

2. **Process User Answers**:
   - Extract answers from "User Reply" sections that the user has filled in
   - Do NOT modify or fill in answers yourself - only read what the user has provided
   - Mark questions as answered based on user-provided answers

3. **Generate Additional Questions** (if needed):
   - Based on user answers, generate follow-up questions if anything is unclear
   - **WRITE TO AI-FLOWS/**: Add new questions to `ai-flows/<feature_name>/plan-questions.md` with "User Reply: __" placeholders
   - Include multiple choice options where applicable
   - Stay within the complexity-based question caps (5/15/25) and retire lower-priority questions if needed
   - Avoid topics covering testing, deployment, documentation updates, or future enhancements
   - Present new questions and wait if clarification needed

4. **Create or Update Plan**:
   - **WRITE TO AI-FLOWS/**: If plan doesn't exist and all critical questions have user answers, create `ai-flows/<feature_name>/plan.md`
   - **WRITE TO AI-FLOWS/**: If plan exists, update `ai-flows/<feature_name>/plan.md` with new information from user answers
   - Ensure plan reflects all clarifications from user responses
   - Confirm the plan respects the line cap (100/200/300) and contains no code snippets or out-of-scope items
   - **WRITE TO AI-FLOWS/**: Update `ai-flows/<feature_name>/todo.md` if plan structure changed (phases added/modified)

### Scenario C: Plan Review Comments Provided
**Indicators:**
- `ai-flows/<feature_name>/review-plan.md` exists with comments
- User has reviewed the plan and provided feedback
- Plan needs to be refined based on review

**Workflow:**
1. **Read Review Comments**:
   - **READ FROM AI-FLOWS/**: Read `ai-flows/<feature_name>/review-plan.md`
   - Understand all feedback and requested changes
   - Identify areas that need clarification or modification

2. **Read Current Plan**:
   - **READ FROM AI-FLOWS/**: Read existing `ai-flows/<feature_name>/plan.md`
   - Understand current plan structure and content

3. **Generate Questions** (if needed):
   - If review comments raise new ambiguities, generate questions
   - Include multiple choice options when applicable
   - Add "User Reply: __" section after each question
   - Stay within the 5/15/25 question caps—merge or drop lower-priority prompts before exceeding them
   - Do not ask about testing, deployment, documentation updates, or future enhancements
   - **WRITE TO AI-FLOWS/**: Add to `ai-flows/<feature_name>/plan-questions.md`
   - Ask user to fill in "User Reply" sections for clarification

4. **Update Plan**:
   - **WRITE TO AI-FLOWS/**: Modify `ai-flows/<feature_name>/plan.md` based on review comments
   - Address all feedback points
   - Ensure plan reflects all requested changes
   - Maintain detailed structure (including phases if applicable) while staying within the line cap and excluding code snippets or out-of-scope topics
   - **WRITE TO AI-FLOWS/**: Update `ai-flows/<feature_name>/todo.md` to reflect any changes in implementation steps

## Detailed Plan Structure

The plan.md file should include (at minimum):

### 1. Feature Header
```
FEATURE: <feature_name>
```

### 2. Executive Summary
- Brief overview of the feature
- Goals and objectives
- Success criteria

### 3. Requirements Analysis
- Functional requirements (detailed)
- Non-functional requirements
- User stories/use cases
- Acceptance criteria

### 4. Technical Analysis
- Current system architecture (relevant parts)
- Affected components/modules
- Dependencies and integrations
- Data models and structures
- API endpoints (if applicable)
- Database changes (if applicable)

### 5. Implementation Strategy
- High-level approach
- Technology choices and rationale
- Design patterns to use
- Architectural decisions

### 6. Phase-Based Breakdown (For Large Features)
If the feature is large or complex, break it into multiple phases:

**Phase Assessment Criteria:**
- Features with 10+ files to create/modify should be broken into phases
- Features with multiple independent components should be phased
- Features that can be incrementally delivered should be phased
- If unsure, ask user if they prefer phased approach

**Phase Structure:**
- **Phase 1: [Phase Name]**
  - Objectives: [What this phase achieves]
  - Scope: [What's included]
  - Deliverables: [What will be completed]
  - Dependencies: [What must exist before this phase]
  - Duration estimate: [If available]
  - Detailed steps: [See section 7 below]

- **Phase 2: [Phase Name]**
  - [Same structure]
  - Dependencies: [May depend on Phase 1]

**Phase Sequencing:**
- Define clear dependencies between phases
- Ensure each phase delivers working functionality that aligns with the agreed scope
- Consider risk mitigation by prioritizing critical/complex phases

### 7. Detailed Implementation Steps
Break down into granular steps (within each phase if phased, or overall if single phase):

- Step 1: [Description]
  - Files to create/modify: [list]
  - Changes needed: [detailed description]
  - Dependencies: [list]

- Step 2: [Description]
  - [Same structure]

**Note:** If feature is broken into phases, organize steps under their respective phase sections and describe behaviors without inserting code snippets.

### 8. File-Level Breakdown
For each file that will be created or modified:
- File path
- Purpose and responsibility
- Key functions/classes/components
- Dependencies and imports

### 9. Integration Points
- How this feature integrates with existing code
- APIs to call or expose
- Database interactions
- External services (if any)

### 10. Edge Cases & Error Handling
- All identified edge cases
- Error scenarios
- Validation requirements
- Error handling strategy

### 11. Risks & Mitigations
- Identified risks
- Mitigation strategies
- Contingency plans

### 12. Open Questions (if any)
- Any remaining ambiguities
- Decisions pending user input

## Question File Structure

The `plan-questions.md` file should organize questions by category. **CRITICAL**: The agent MUST NOT fill in answers. Only the user should provide answers in the "User Reply" section.

```markdown
# Planning Questions for FEATURE: <feature_name>

## Requirement Questions
1. [Question text]
   - Options (if applicable):
     - a) [Option 1]
     - b) [Option 2]
     - c) [Option 3]
     - d) [Other - please specify]
   - User Reply: __
   
2. [Question text]
   - Options (if applicable):
     - a) [Option 1]
     - b) [Option 2]
   - User Reply: __

## Technical Questions
1. [Question text]
   - Options (if applicable):
     - a) [Option 1]
     - b) [Option 2]
     - c) [Option 3]
   - User Reply: __

## Design Questions
1. [Question text]
   - Options (if applicable):
     - a) [Option 1]
     - b) [Option 2]
   - User Reply: __

[... continue with other categories]
```

**Important Guidelines:**
- Always include a "User Reply: __" section after each question where the user will fill in their answer
- Provide multiple choice options when applicable to make it easier for users to select answers
- For open-ended questions, you may omit options but still include "User Reply: __"
- The agent MUST NOT fill in the "User Reply" sections - these are reserved for the user
- When processing answers, read from the "User Reply" sections that users have filled in
- Keep the total question count within the complexity-based limits (5/15/25)
- Exclude questions about testing, deployment, documentation updates, or future enhancements

## Review File Structure

The `review-plan.md` file should include:

```markdown
# Plan Review Comments for FEATURE: <feature_name>

## General Feedback
[Review comments]

## Section-Specific Feedback

### [Section Name]
[Specific comments about this section]

## Action Items
- [ ] Item 1
- [ ] Item 2
```

## Implementation Todos File Structure

The `todo.md` file should contain actionable implementation todos extracted from the plan. Keep todos high-level, avoid all code snippets, and exclude testing, deployment, documentation updates, and future enhancement work. Structure as follows:

```markdown
# Implementation Todos for FEATURE: <feature_name>

## Overview
Brief summary of all todos and their organization.

## Phase-Based Todos (if feature is phased)

### Phase 1: [Phase Name]
- [ ] Todo 1: [Description]
  - Files: [list of files]
  - Priority: [High/Medium/Low]
  - Dependencies: [any blocking todos]
  - Estimated complexity: [Simple/Medium/Complex]
  
- [ ] Todo 2: [Description]
  - [Same structure]

### Phase 2: [Phase Name]
- [ ] Todo 1: [Description]
  - [Same structure]

## All Todos (Flat List - Alternative/Additional Format)

If not using phases, or for a complete overview:

### High Priority
- [ ] Todo 1
- [ ] Todo 2

### Medium Priority
- [ ] Todo 3
- [ ] Todo 4

### Low Priority
- [ ] Todo 5

## File-Specific Todos

### [File Path 1]
- [ ] Create file with [components/functions]
- [ ] Add [specific functionality]

### [File Path 2]
- [ ] Modify [existing code]
- [ ] Add [new features]

## Notes
- Todos should be specific and actionable
- Each todo should reference relevant plan sections
- Mark todos as complete during implementation (outside planning phase)
```

**Important:** 
- Todos should be extracted directly from the detailed implementation steps in the plan
- If the plan is phased, organize todos by phase
- Each todo should be specific enough to be actionable
- Include file paths, dependencies, and priorities
- Do not include testing, deployment, documentation updates, or future enhancements in todos

## Execution Protocol

### Step 1: Determine Scenario
When plan mode is invoked:
1. Check if `ai-flows/` directory exists, if not create it (this is the ONLY directory creation allowed)
2. Check for existing feature directories in `ai-flows/`
3. Determine which scenario (A, B, or C) applies
4. Follow the appropriate workflow

### Step 2: File Discovery
- **READ ONLY FROM <ADD_YOUR_REPO>-* FOLDERS**: Use codebase search to find all relevant files in `<ADD_YOUR_REPO>-backend/`, `<ADD_YOUR_REPO>-ui/`, and `<ADD_YOUR_REPO>-admin/`
- Read key files from <ADD_YOUR_REPO>-* folders to understand the project structure
- Identify patterns, conventions, and architectural decisions
- Map dependencies and relationships
- **DO NOT MODIFY**: Only read files from <ADD_YOUR_REPO>-* folders, never edit or create files there

### Step 3: Question Generation
- Generate questions based on:
  - Gaps in understanding from file analysis (from <ADD_YOUR_REPO>-* folders)
  - Standard requirement categories
  - Technical considerations
  - User experience aspects
- For each question:
  - Include multiple choice options when applicable
  - Add "User Reply: __" section where the user will fill in their answer
  - Do NOT fill in answers yourself
- Stay within the 5/15/25 question caps and remove or consolidate lower-priority prompts before exceeding them
- Skip topics covering testing, deployment, documentation updates, or future enhancements
- Organize questions by category
- **WRITE TO AI-FLOWS/**: Write to `ai-flows/<feature_name>/plan-questions.md` with the proper format including "User Reply: __" placeholders

### Step 4: Plan Generation/Update
- Only proceed when sufficient information is available
- Assess feature complexity and determine if phase breakdown is needed
- Create comprehensive, detailed plan
- If feature is large/complex, organize into phases with detailed plans for each
- Include file-level details (referencing files in <ADD_YOUR_REPO>-* folders that will be modified during implementation)
- Make it actionable and specific
- Ensure plan text stays within the 100/200/300 line cap, avoids all code snippets, and excludes testing/deployment/documentation/future enhancement content
- **WRITE TO AI-FLOWS/**: Save to `ai-flows/<feature_name>/plan.md`

### Step 4a: Todo Extraction
- Extract all actionable implementation todos from the plan
- Organize todos by phase (if phased) or by priority/type
- Include file paths (in <ADD_YOUR_REPO>-* folders), dependencies, and priorities for each todo
- **WRITE TO AI-FLOWS/**: Create `ai-flows/<feature_name>/todo.md` with structured todos
- Ensure todos are specific and actionable
- Keep todos free of code snippets and omit testing, deployment, documentation updates, and future enhancements

### Step 5: Present Results
- Show the plan or questions to user
- Wait for user input (answers or review)
- Be ready to iterate

## Quality Checklist

Before finalizing a plan, ensure:
- [ ] Feature name is correctly formatted at the top
- [ ] All relevant files have been analyzed
- [ ] All questions have been asked or answered
- [ ] Total questions remain within the 5/15/25 cap
- [ ] Plan is detailed enough to implement without further clarification
- [ ] Feature complexity assessed - phases defined if needed
- [ ] Every file that will be created/modified is listed
- [ ] All edge cases are considered
- [ ] Integration points are clear
- [ ] Plan text stays within the allowed line cap and contains zero code snippets
- [ ] No assumptions are made without asking
- [ ] Implementation todos extracted to `todo.md`
- [ ] Todos are organized by phase (if applicable) and priority
- [ ] Todos exclude testing, deployment, documentation updates, future enhancements, and code snippets

## Important Notes

1. **Never Skip Questions**: Even if something seems obvious, ask to confirm understanding
2. **Be Thorough**: Better to over-plan than under-plan
3. **Stay in Read-Only Mode for Codebase**: This is planning, not implementation - never modify <ADD_YOUR_REPO>-* folders
4. **Write Planning Artifacts**: Create and update files only in `ai-flows/<feature_name>/` folder
5. **Document Everything**: All questions, answers, and decisions should be recorded in ai-flows/ folder
6. **Iterate as Needed**: Planning is iterative - refine based on feedback by updating files in ai-flows/ folder
7. **Respect Constraints**: Always honor the question caps, plan line limits, and no-code-snippet rule while excluding testing, deployment, documentation updates, and future enhancements

## Example Workflow

```
User: "Plan a user authentication feature"

Assistant:
1. Creates ai-flows/user-authentication/ directory (WRITE to ai-flows/)
2. Analyzes codebase for authentication patterns, user models, etc. (READ from <ADD_YOUR_REPO>-backend/, <ADD_YOUR_REPO>-ui/)
3. Generates comprehensive questions in ai-flows/user-authentication/plan-questions.md (WRITE to ai-flows/)
4. Presents questions to user

User: Fills in "User Reply" sections in plan-questions.md

Assistant:
1. Reads user-provided answers from "User Reply" sections in ai-flows/user-authentication/plan-questions.md (READ from ai-flows/)
2. Creates detailed ai-flows/user-authentication/plan.md with all implementation steps (in phases if large) based on user answers (WRITE to ai-flows/)
3. Extracts todos from plan and creates ai-flows/user-authentication/todo.md (WRITE to ai-flows/)
4. Presents plan and todos

User: Reviews plan and adds comments to review-plan.md

Assistant:
1. Reads ai-flows/user-authentication/review-plan.md (READ from ai-flows/)
2. Updates ai-flows/user-authentication/plan.md based on feedback (WRITE to ai-flows/)
3. Presents refined plan
```

---

**Remember**: This is a planning phase. 
- **READ-ONLY FOR CODEBASE**: No files in `<ADD_YOUR_REPO>-backend/`, `<ADD_YOUR_REPO>-ui/`, or `<ADD_YOUR_REPO>-admin/` should be modified or created
- **WRITE-ONLY FOR PLANNING**: Only write and update planning artifacts in `ai-flows/<feature_name>/` folder
- **NO CODE IMPLEMENTATION**: No code should be written in <ADD_YOUR_REPO>-* folders during planning phase
- **ONLY PLANNING DOCUMENTATION**: Only create and update planning documentation (plan.md, plan-questions.md, review-plan.md, todo.md) in ai-flows/ folder
