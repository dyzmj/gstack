```markdown
# gstack Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill teaches the core development patterns, workflows, and coding conventions used in the `gstack` TypeScript codebase. `gstack` is a modular system for managing "skills"—discrete, reusable workflows and tools—without relying on a specific framework. The repository emphasizes clarity, maintainability, and automation, with strong conventions around code style, documentation, and workflow automation.

## Coding Conventions

- **Language:** TypeScript (no framework)
- **File Naming:** Use `camelCase` for files and folders.
  - Example: `mySkill.ts`, `sharedResolver.ts`
- **Import Style:** Use relative imports.
  - Example:
    ```typescript
    import { myHelper } from './myHelper';
    ```
- **Export Style:** Use named exports.
  - Example:
    ```typescript
    export function myFunction() { ... }
    export const MY_CONST = 42;
    ```
- **Commit Messages:** Use [Conventional Commits](https://www.conventionalcommits.org/) with prefixes like `feat`, `fix`, `docs`.
  - Example: `feat: add support for multi-host skill generation`

## Workflows

### skill-template-update-and-regeneration
**Trigger:** When a change is made to skill templates, shared resolvers, or preamble logic that should affect all skills.  
**Command:** `/regen-skills`

1. Edit one or more `SKILL.md.tmpl` files or `scripts/resolvers/*.ts` (e.g., add new section, update preamble, fix bug).
2. Run the doc generation script:
   ```sh
   bun run gen:skill-docs
   ```
3. Optionally update related tests to reflect new template/resolver behavior.
4. Commit regenerated `SKILL.md` files along with template/resolver changes.

**Files involved:**  
- `*/*/SKILL.md.tmpl`
- `scripts/resolvers/*.ts`
- `*/*/SKILL.md`
- `test/gen-skill-docs.test.ts`
- `test/skill-validation.test.ts`

---

### version-bump-and-changelog-update
**Trigger:** When a new feature, fix, or release is ready to land on main or a feature branch.  
**Command:** `/bump-version`

1. Edit the `VERSION` file to increment the version number.
2. Update `CHANGELOG.md` with a new entry summarizing changes.
3. Sync `package.json` version to match `VERSION`.
4. Commit `VERSION`, `CHANGELOG.md`, and `package.json` together.

**Files involved:**  
- `VERSION`
- `CHANGELOG.md`
- `package.json`

---

### add-or-update-shared-resolver
**Trigger:** When introducing new shared logic (e.g., new workflow step, audit, or integration) to be used by multiple skills.  
**Command:** `/add-resolver`

1. Implement or update a resolver function in `scripts/resolvers/*.ts`.
   ```typescript
   // scripts/resolvers/myResolver.ts
   export function myResolver(args) { ... }
   ```
2. Register the resolver in `scripts/resolvers/index.ts`.
   ```typescript
   export * from './myResolver';
   ```
3. Add or update template placeholders (e.g., `{{MY_RESOLVER}}`) in one or more `SKILL.md.tmpl` files.
4. Regenerate affected `SKILL.md` files.
5. Add or update tests for the resolver and template output.

**Files involved:**  
- `scripts/resolvers/*.ts`
- `scripts/resolvers/index.ts`
- `*/*/SKILL.md.tmpl`
- `*/*/SKILL.md`
- `test/gen-skill-docs.test.ts`

---

### add-new-skill
**Trigger:** When a new workflow, tool, or capability is needed as a first-class skill.  
**Command:** `/add-skill`

1. Create a new directory for the skill (e.g., `new-skill/`).
2. Add `SKILL.md.tmpl` and generate `SKILL.md` using the doc generation script.
3. Add the skill to documentation (`README.md`, `docs/skills.md`).
4. Update setup scripts and symlink logic if needed.
5. Add tests for the new skill's template and integration.

**Files involved:**  
- `new-skill/SKILL.md.tmpl`
- `new-skill/SKILL.md`
- `README.md`
- `docs/skills.md`
- `setup`
- `test/gen-skill-docs.test.ts`

---

### security-hardening-and-audit-remediation
**Trigger:** When a security audit (manual, Snyk, Socket, adversarial review) finds vulnerabilities or best practices to enforce.  
**Command:** `/security-audit`

1. Identify affected files (binaries, extension scripts, server code, shell scripts).
2. Apply fixes (e.g., input validation, path sanitization, allowlists, escaping, RLS policy changes).
3. Add or update regression/security tests for the fixed issues.
4. Update documentation to reflect new security posture.
5. Bump version and update changelog.

**Files involved:**  
- `bin/*`
- `browse/src/*.ts`
- `extension/*.js`
- `supabase/*`
- `test/*security*.test.ts`
- `test/telemetry.test.ts`
- `CHANGELOG.md`
- `VERSION`

---

### test-suite-expansion-and-touchfile-classification
**Trigger:** When adding new features, fixing bugs, or improving test reliability and CI performance.  
**Command:** `/add-test`

1. Write new or update existing test files (unit, integration, E2E).
   ```typescript
   // Example test
   import { myFunction } from '../myFunction';

   test('should return correct value', () => {
     expect(myFunction()).toBe(42);
   });
   ```
2. Add or update touchfile entries and tier classifications (gate/periodic) in `test/helpers/touchfiles.ts`.
3. Update or add test runner scripts (`package.json`).
4. Update documentation (`CLAUDE.md`, `test/skill-llm-eval.test.ts`) to reflect new test structure.
5. Commit test files, touchfile helpers, and related docs.

**Files involved:**  
- `test/*.test.ts`
- `test/helpers/touchfiles.ts`
- `test/skill-llm-eval.test.ts`
- `package.json`
- `CLAUDE.md`

---

### platform-compatibility-and-multi-host-support
**Trigger:** When expanding gstack to work with new AI code agents or platforms.  
**Command:** `/add-platform`

1. Update `scripts/gen-skill-docs.ts` to support new host types.
2. Add host-specific output directories and frontmatter logic.
3. Update setup scripts to generate/install skills for new hosts.
4. Add or update tests for host-specific generation and install.
5. Update documentation (`README.md`, `TODOS.md`) for new platform support.

**Files involved:**  
- `scripts/gen-skill-docs.ts`
- `setup`
- `.factory/skills/*/SKILL.md`
- `README.md`
- `TODOS.md`
- `test/gen-skill-docs.test.ts`

---

### design-mockup-pipeline-update
**Trigger:** When improving or fixing the design-to-mockup pipeline, feedback collection, or artifact persistence.  
**Command:** `/update-design-pipeline`

1. Update `design/src/*` (e.g., `commands.ts`, `compare.ts`, `serve.ts`) for new features or fixes.
2. Update `scripts/resolvers/design.ts` and `SKILL.md.tmpl` files to use new or improved resolvers.
3. Regenerate affected `SKILL.md` files.
4. Update or add tests for design feedback loop, gallery, and artifact persistence.
5. Update documentation (`docs/designs/DESIGN_SHOTGUN.md`, `DESIGN_TOOLS_V1.md`).

**Files involved:**  
- `design/src/*.ts`
- `scripts/resolvers/design.ts`
- `design-shotgun/SKILL.md.tmpl`
- `design-shotgun/SKILL.md`
- `test/gen-skill-docs.test.ts`
- `test/helpers/touchfiles.ts`
- `docs/designs/DESIGN_SHOTGUN.md`
- `docs/designs/DESIGN_TOOLS_V1.md`

---

### browser-extension-and-sidebar-agent-enhancement
**Trigger:** When adding new sidebar features, improving UX, or fixing browser integration bugs.  
**Command:** `/update-sidebar`

1. Update extension JS/CSS/HTML files for new features or UI changes.
2. Update `browse/src/server.ts`, `sidebar-agent.ts`, `browser-manager.ts` for backend/agent logic.
3. Update or add tests for sidebar agent, UX, security, and E2E flows.
4. Update documentation (`BROWSER.md`, `README.md`) for new sidebar/browser features.
5. Bump version and update changelog.

**Files involved:**  
- `extension/*.js`
- `extension/*.css`
- `extension/*.html`
- `browse/src/sidebar-agent.ts`
- `browse/src/server.ts`
- `browse/src/browser-manager.ts`
- `test/skill-e2e-sidebar.test.ts`
- `test/helpers/touchfiles.ts`
- `BROWSER.md`
- `README.md`
- `CHANGELOG.md`
- `VERSION`

---

## Testing Patterns

- **Framework:** [Jest](https://jestjs.io/)
- **Test File Pattern:** `*.test.ts`
- **Placement:** All test files are located in the `test/` directory or alongside the code.
- **Example Test:**
  ```typescript
  import { myFunction } from '../src/myFunction';

  test('returns expected result', () => {
    expect(myFunction()).toBe('expected');
  });
  ```

- **E2E and Tiered Tests:** Use `test/helpers/touchfiles.ts` for test classification (e.g., gate, periodic).
- **Test Coverage:** Tests are expanded and updated with new features, bug fixes, or CI improvements.

## Commands

| Command              | Purpose                                                      |
|----------------------|--------------------------------------------------------------|
| /regen-skills        | Regenerate all SKILL.md files from updated templates         |
| /bump-version        | Bump project version and update changelog                    |
| /add-resolver        | Add or update a shared resolver for skill templates          |
| /add-skill           | Create and integrate a new skill into the gstack ecosystem   |
| /security-audit      | Apply security fixes and audit remediations                  |
| /add-test            | Add or expand test coverage and update test classifications  |
| /add-platform        | Add support for a new agent host or platform                 |
| /update-design-pipeline | Update the design-to-mockup pipeline and feedback loop    |
| /update-sidebar      | Enhance browser extension and sidebar agent features         |
```
