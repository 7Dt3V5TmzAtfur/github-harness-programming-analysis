# Verification

This file records public-ready checks for this repository.

## Current Verification

| Check | Command or surface | Expected result |
|---|---|---|
| Markdown relative links | local link checker | all relative links exist |
| Public boundary scan | `rg` scan for private paths and old video-resource wording | no hits |
| SVG assets | XML parse + file existence check | all SVG files parse |
| GitHub metadata | `gh repo view` | visibility `PUBLIC`, description and topics set |
| Remote hash proof | `git ls-remote origin refs/heads/main` | remote hash equals local HEAD |

## Public Boundary

This repository should contain reusable workflow instructions, Skills, prompts, GitHub templates, checklists, and demo material.

It should not contain private project material, credentials, personal workspace paths, private registry data, account-specific automation, or unrelated media assets.

## Notes

Visual assets are SVG sources in `assets/`. If PNG social preview assets are added later, record the local render command and image verification result here.
