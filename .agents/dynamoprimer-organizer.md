---
name: dynamoprimer-organizer
description: Organizes Dynamo Primer repo structure — chapters, images, and assets.
argument-hint: "[chapter-path or 'audit']"
allowed-tools: Read, Glob, Grep, Edit, Bash
context: fork
---

## Current Repo Structure
!`find . -type d -name "images" 2>/dev/null | head -30`

## All Image Assets
!`find . \( -name "*.png" -o -name "*.jpg" -o -name "*.gif" -o -name "*.svg" \) 2>/dev/null | head -30`

# Dynamo Primer Organization Skill

## Purpose
Maintain a consistent, scalable, and maintainable structure for the Dynamo Primer across both:
- GitHub repository (source of truth)
- GitBook (published documentation)

This skill enforces:
- Logical chapter structure
- Localized asset management
- Clean, non-duplicated media usage
- Long-term maintainability

---

## Core Principles

### 1. Chapter-Centric Organization
- Each chapter must be self-contained
- All assets (images, diagrams, media) belong inside the chapter folder
- Avoid global/shared asset folders unless absolutely necessary, e.g. resources are shared between chapters

**Preferred structure:**

- Chapter [e.g., 5_essential_nodes_and_concepts]
    - Subchapter [e.g., 5-2_geometry-for-computational-design]
        - Images folder
            - Image file

---

### 2. No Global "Assets Dump"
- Do NOT store images in a single `/assets` or non-chapter-specific `/images` root folder
- Exception: only for truly shared, reusable global assets (rare)

If found:
- Relocate assets into their respective chapter folders
- Update all references accordingly

---

### 3. Image Deduplication
Before adding any image:
1. Search the repo for similar or identical images
2. Reuse existing assets when possible
3. If duplication exists:
   - Keep the best-quality or most canonical version
   - Remove duplicates
   - Update references

---

### 4. Image Naming & Versioning
Use descriptive, consistent naming:

**Format:**

topic-concept-variant.png

Ensure there are no spaces in file names. Replace spaces with a hyphen: -

Examples:
- `vectors-cross-product-diagram.png`
- `list-filter-example-01.png`

Versioning:
- Prefer updating existing files over creating duplicates like `image-final-v2.png`
- If versioning is necessary:

example-01-v2.png


---

### 5. Relative Referencing Only
- Always use relative paths in markdown:
  `![alt text](../images/topic-concept.png)`
- Never use:
  - Absolute paths
  - External image hosting (unless required)

---

### 6. GitBook Consistency
Ensure:
- Folder structure mirrors GitBook hierarchy
- Navigation reflects actual repo structure
- No orphaned files or unused assets

---

### 7. Pull Request Review Checklist

When reviewing PRs, verify:

- [ ] Files placed in correct chapter folder
- [ ] Images stored locally (not global)
- [ ] No duplicate images introduced
- [ ] Naming conventions followed
- [ ] Relative paths used correctly
- [ ] No unused assets added
- [ ] Structure aligns with existing chapters

---

## Refactoring Workflow

When cleaning existing content:

1. Identify misplaced assets
2. Move assets into correct chapter folders
3. Update all references
4. Remove duplicates
5. Normalize naming
6. Validate links and rendering

---

## Anti-Patterns (Reject or Fix)

- Centralized `/assets` or `/images` dumping ground
- Duplicate images with different names
- Vague file names (`image1.png`, `final.png`)
- File names with spaces in them
- Broken or absolute links
- Chapter content depending on external folders

---

## Output Expectations

When applying this skill:
- Propose clear structural changes
- Suggest file moves and renames
- Highlight duplicates and inconsistencies
- Provide exact updated paths and references
- Ensure minimal disruption to existing content

---

## Optional Enhancements

If applicable:
- Suggest automation scripts for validation (e.g., duplicate detection)
- Recommend linting rules for markdown/image paths
- Identify opportunities for shared components (used sparingly)
