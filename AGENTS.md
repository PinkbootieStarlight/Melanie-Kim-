# AI Agent Conventions

This file defines how AI tools (Claude, ChatGPT, Copilot, or others) should work within this repository. `CLAUDE.md` points here so any Claude session picks this up automatically.

## How I want explanations

Plain language over jargon — I'm bringing 25+ years of clinical and healthcare-operations experience, not a finance or CS background, so explain the reasoning behind a formula or approach, not just the steps to run it. Show your work rather than just the answer. Ask before assuming anything about my data, my numbers, or what I intend an analysis to conclude.

## What AI may draft

First-pass drafts of briefs (`docs/briefs/`) and capability write-ups (`spec.md`) for me to revise; README indexes and folder `README.md` files; data-cleaning and formatting scripts; light formatting/cleanup of notes I've already written in my own words.

## What AI should not do

- Do not fabricate data, sources, citations, or results.
- Do not write final recommendation memos (`docs/decisions/`) without my review and edits.
- Do not commit or push changes on my behalf — I control git through GitHub Desktop.
- Do not include real patient information, PHI, or any identifiable clinical data anywhere in this repository.
- Do not invent dates, credentials, or biographical details about me — ask me for the specifics instead.

## Style notes

Direct and specific over hedged or vague. Use standard business/finance and healthcare-operations terminology rather than oversimplifying. Name the method or formula being used (e.g., "P = MC") rather than describing it only in prose.
