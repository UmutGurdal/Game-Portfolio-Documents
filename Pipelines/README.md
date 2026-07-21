# Pipelines

> [!NOTE]
> This folder is reserved for build, release, and automation notes. The portfolio games were shipped as Unity Android (IL2CPP) products with manual/editor-assisted release flows rather than a public CI config in this repository.

## Current release practices (high level)

- Unity Android player builds (ARM, IL2CPP where configured per project)
- Store versioning coordinated with in-game remote update checks
- Mediation/adapter dependency updates as part of release prep (ads stack)
- Privacy/UMP verification before production ad serving
- Cloud save and login smoke checks on GPGS-enabled builds

## Planned docs

- Android release checklist
- Ads mediation upgrade checklist
- Save migration verification steps

Contributions/notes can be added here as markdown case studies without publishing proprietary project files or keystores.
