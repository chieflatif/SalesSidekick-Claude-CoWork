---
_schema: 1
type: index
generated: {{CREATED_DATE}}
entity_counts:
  companies: 0
  deals: 0
  contacts: 0
  call_notes: 0
  tasks: 0
---

## Deals (Active)

| id | company | contacts | stage | amount | close_date | probability | health | meddpicc | signals | last_activity | next_step |
|----|---------|----------|-------|--------|------------|-------------|--------|----------|---------|---------------|-----------|

## Deals (Closed)

| id | company | stage | amount | close_date | status | closed_reason |
|----|---------|-------|--------|------------|--------|---------------|

## Companies

| id | name | industry | size | region | deals | contacts | last_researched |
|----|------|----------|------|--------|-------|----------|----------------|

## Contacts

| id | name | company | deals | title | role | engagement | last_contact | email |
|----|------|---------|-------|-------|------|------------|-------------|-------|

## Signals Active

| deal | signal | severity | detected | evidence |
|------|--------|----------|----------|----------|

## Tasks Active

| id | deal | company | description | due_date | status |
|----|------|---------|-------------|----------|--------|
