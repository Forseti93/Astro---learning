# StitchMCP Usage Skill

This skill provides guidelines and best practices for interacting with the StitchMCP server to manage UI designs, screens, and design systems.

## Overview
StitchMCP is a platform for generating and managing UI designs and frontend code. It organizes work into **Projects**, which contain **Screens** and **Design Systems**.

## Core Concepts
- **Project**: A container for UI designs and code. Identified by a `projectId`.
- **Screen**: A single UI page or component within a project. Identified by a `screenId`.
- **Design System**: A set of visual tokens (colors, typography, shapes) applied to screens. Identified by an `assetId`.

## Workflow & Guidelines

### 1. Project Management
- **List Projects**: Use `list_projects` to find existing work.
- **Create Project**: Use `create_project` to start a new design task. Always save the `projectId` returned.
- **Get Project**: Use `get_project` to see screens and design systems associated with a project.

### 2. Design System Setup
- **Create/Update**: Use `create_design_system` or `update_design_system`.
- **Tokens**: Configure `palette`, `typography`, `shape`, and `appearance` (light/dark mode).
- **Design MD**: Use the `design_md` field for free-form instructions to the generation model.
- **Applying**: After creating/updating a design system, use `apply_design_system` to update existing screens.

### 3. Screen Generation & Editing
- **Generate from Text**: Use `generate_screen_from_text` for initial creation. 
- **Edit Screens**: Use `edit_screens` to modify existing screens based on a prompt.
- **Generate Variants**: Use `generate_variants` to explore different design options for existing screens.

> [!IMPORTANT]
> **Patience is required**: Screen generation and editing can take several minutes. 
> **DO NOT RETRY** immediately if a connection error occurs. The process often continues on the server.
> Use `get_screen` or `list_screens` after a few minutes to check if the new screen has appeared.

### 4. Selection & Application
- When editing or applying styles, you must provide `selectedScreenIds` or `selectedScreenInstances`.
- Fetch these IDs from the project details (`get_project`) or by listing screens (`list_screens`).

## Example Usage Pattern

1. **Initialize**: `create_project(title="E-commerce App")`
2. **Define Style**: `create_design_system(projectId="...", designSystem={...})`
3. **Generate Landing**: `generate_screen_from_text(projectId="...", prompt="A modern landing page for a sneaker store")`
4. **Refine**: `edit_screens(projectId="...", selectedScreenIds=["..."], prompt="Make the CTA button larger and bright orange")`
5. **Check Progress**: If the tool times out, wait 1-2 minutes and call `list_screens(projectId="...")`.
