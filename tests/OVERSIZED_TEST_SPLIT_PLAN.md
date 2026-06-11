# Oversized Test File Split Plan

## Purpose

This document plans future oversized test-file splits using current repo data.
It does not move files, rewrite assertions, extract helpers, or change CI.

## Roadmap context

- Issue: #3983
- Parent tracker: #2523
- Follows #3973 / #3982, the report-only order-sensitivity diagnostics slice.

## Methodology

Metrics were generated from the current test tree using:

- physical line counts for every recursive `test_*.py` file under `tests/`;
- AST counts for `test_*` functions and `Test*` classes;
- one `pytest --collect-only -q tests` run to count collected items per file;
- current taxonomy classification from `tests._taxonomy.classify_test_path`; and
- static setup-signal scans for route/API, DB/session, import-state, security, filesystem, subprocess/script, async/threading, and UI/static indicators.

Static signals are not proof of risk. They are review prompts.
Future split PRs must still inspect each file manually before editing.

## Current summary

- test files scanned: 541
- collected pytest items counted: 3263
- large-file threshold: 300 lines
- large-collected threshold: 20 collected items

Area distribution:

| Value | Files |
|---|---:|
| cli | 28 |
| helpers | 1 |
| js | 36 |
| routes | 22 |
| security | 71 |
| services | 134 |
| uncategorized | 212 |
| unit | 37 |

Sub-area distribution:

| Value | Files |
|---|---:|
| api | 5 |
| atomic | 3 |
| auth | 8 |
| calendar | 9 |
| cli | 28 |
| confinement | 5 |
| cookbook | 10 |
| document | 11 |
| email | 11 |
| embedding | 3 |
| gallery | 4 |
| history | 3 |
| js | 36 |
| llm | 15 |
| mcp | 8 |
| memory | 13 |
| nondict | 7 |
| nonstring | 22 |
| owner | 13 |
| owner_scope | 23 |
| parse | 4 |
| provider | 6 |
| research | 16 |
| route | 6 |
| routes | 9 |
| scheduler | 3 |
| scope | 5 |
| security | 9 |
| session | 16 |
| ssrf | 2 |
| webhook | 2 |
| xss | 5 |

Values below 2 files: 221 values covering 221 files.

## Top files by collected pytest items

| File | Lines | Collected tests | Test defs | Test classes | Area | Sub-area | Signals |
|---|---:|---:|---:|---:|---|---|---|
| `tests/test_model_routes.py` | 1662 | 133 | 110 | 10 | routes | routes | route/api, db/session, import-state, async/threading |
| `tests/test_security_regressions.py` | 1203 | 91 | 67 | 0 | security | security | route/api, db/session, import-state, security, filesystem, async/threading, ui/static |
| `tests/test_provider_classification.py` | 188 | 67 | 21 | 4 | services | provider | - |
| `tests/test_shell_routes.py` | 460 | 62 | 47 | 8 | routes | routes | route/api, import-state, filesystem |
| `tests/test_cookbook_helpers.py` | 819 | 59 | 59 | 0 | services | cookbook | route/api, filesystem, subprocess/script, async/threading |
| `tests/test_pr_blocker_audit.py` | 964 | 58 | 58 | 0 | uncategorized | pr_blocker_audit | import-state, security, filesystem |
| `tests/test_provider_endpoints.py` | 241 | 58 | 18 | 1 | services | provider | subprocess/script |
| `tests/test_agent_loop.py` | 458 | 51 | 51 | 5 | uncategorized | agent_loop | db/session, import-state |
| `tests/test_service_health.py` | 472 | 47 | 42 | 0 | uncategorized | service_health | async/threading |
| `tests/test_run_focus.py` | 399 | 47 | 44 | 0 | uncategorized | run_focus | security, filesystem, subprocess/script, ui/static |
| `tests/test_endpoint_probing.py` | 411 | 34 | 30 | 6 | uncategorized | endpoint_probing | route/api, db/session, import-state |
| `tests/test_llm_core_anthropic_temp_omit.py` | 94 | 32 | 6 | 0 | services | llm | db/session |
| `tests/test_provider_detection.py` | 146 | 31 | 31 | 5 | services | provider | - |
| `tests/test_model_context.py` | 251 | 30 | 30 | 4 | uncategorized | model_context | db/session, import-state |
| `tests/test_endpoint_resolver.py` | 148 | 30 | 30 | 6 | uncategorized | endpoint_resolver | - |
| `tests/test_embedding_lanes.py` | 1104 | 29 | 29 | 0 | services | embedding | filesystem |
| `tests/test_upload_limits_centralized.py` | 110 | 29 | 5 | 0 | uncategorized | upload_limits_centralized | import-state, filesystem |
| `tests/test_rename_user_owner_sync.py` | 686 | 26 | 26 | 0 | security | owner | route/api, db/session, import-state, filesystem, async/threading |
| `tests/test_helpers_import_state.py` | 426 | 26 | 26 | 0 | helpers | helpers | route/api, db/session, import-state |
| `tests/test_chat_helpers.py` | 220 | 26 | 13 | 0 | uncategorized | chat_helpers | route/api |
| `tests/test_taxonomy.py` | 145 | 26 | 16 | 0 | uncategorized | taxonomy | security, ui/static |
| `tests/test_llm_core_temperature.py` | 127 | 26 | 11 | 0 | services | llm | - |
| `tests/test_review_regressions.py` | 902 | 25 | 25 | 0 | uncategorized | review_regressions | route/api, db/session, import-state, filesystem, async/threading |
| `tests/test_tool_path_confinement.py` | 282 | 24 | 24 | 0 | security | confinement | import-state, filesystem, async/threading |
| `tests/test_copilot.py` | 170 | 23 | 16 | 0 | uncategorized | copilot | - |
| `tests/test_research_utils.py` | 97 | 23 | 23 | 2 | services | research | - |
| `tests/test_api_chat_security.py` | 401 | 22 | 8 | 0 | security | security | route/api, db/session, import-state, filesystem, async/threading |
| `tests/test_platform_compat.py` | 317 | 21 | 21 | 0 | uncategorized | platform_compat | import-state, filesystem, subprocess/script |
| `tests/test_prompt_security.py` | 203 | 21 | 21 | 0 | security | security | - |
| `tests/test_null_owner_gates.py` | 342 | 20 | 20 | 0 | security | owner | route/api, db/session, import-state |

## Top files by physical line count

| File | Lines | Collected tests | Test defs | Test classes | Area | Sub-area | Signals |
|---|---:|---:|---:|---:|---|---|---|
| `tests/test_model_routes.py` | 1662 | 133 | 110 | 10 | routes | routes | route/api, db/session, import-state, async/threading |
| `tests/test_security_regressions.py` | 1203 | 91 | 67 | 0 | security | security | route/api, db/session, import-state, security, filesystem, async/threading, ui/static |
| `tests/test_embedding_lanes.py` | 1104 | 29 | 29 | 0 | services | embedding | filesystem |
| `tests/test_pr_blocker_audit.py` | 964 | 58 | 58 | 0 | uncategorized | pr_blocker_audit | import-state, security, filesystem |
| `tests/test_review_regressions.py` | 902 | 25 | 25 | 0 | uncategorized | review_regressions | route/api, db/session, import-state, filesystem, async/threading |
| `tests/test_cookbook_helpers.py` | 819 | 59 | 59 | 0 | services | cookbook | route/api, filesystem, subprocess/script, async/threading |
| `tests/test_rename_user_owner_sync.py` | 686 | 26 | 26 | 0 | security | owner | route/api, db/session, import-state, filesystem, async/threading |
| `tests/test_api_token_routes.py` | 504 | 14 | 14 | 0 | routes | api_routes | route/api, db/session, import-state, async/threading |
| `tests/test_email_owner_scope.py` | 474 | 9 | 9 | 0 | security | owner_scope | route/api, db/session, filesystem, async/threading |
| `tests/test_service_health.py` | 472 | 47 | 42 | 0 | uncategorized | service_health | async/threading |
| `tests/test_kv_cache_invalidation_2927.py` | 463 | 8 | 8 | 0 | uncategorized | kv_cache_invalidation_2927 | route/api, db/session, import-state, async/threading |
| `tests/test_shell_routes.py` | 460 | 62 | 47 | 8 | routes | routes | route/api, import-state, filesystem |
| `tests/test_agent_loop.py` | 458 | 51 | 51 | 5 | uncategorized | agent_loop | db/session, import-state |
| `tests/test_helpers_import_state.py` | 426 | 26 | 26 | 0 | helpers | helpers | route/api, db/session, import-state |
| `tests/test_endpoint_owner_scope_followup.py` | 414 | 11 | 11 | 0 | security | owner_scope | route/api, db/session, filesystem |
| `tests/test_endpoint_probing.py` | 411 | 34 | 30 | 6 | uncategorized | endpoint_probing | route/api, db/session, import-state |
| `tests/test_imap_leak_fixes.py` | 404 | 15 | 15 | 0 | uncategorized | imap_leak_fixes | route/api, db/session, security, filesystem |
| `tests/test_api_chat_security.py` | 401 | 22 | 8 | 0 | security | security | route/api, db/session, import-state, filesystem, async/threading |
| `tests/test_upload_handler_atomicity.py` | 401 | 9 | 9 | 0 | uncategorized | upload_handler_atomicity | filesystem, async/threading |
| `tests/test_run_focus.py` | 399 | 47 | 44 | 0 | uncategorized | run_focus | security, filesystem, subprocess/script, ui/static |
| `tests/test_auth_regressions.py` | 375 | 15 | 15 | 0 | security | auth | route/api, db/session, import-state, async/threading |
| `tests/test_companion_readonly.py` | 372 | 16 | 16 | 0 | uncategorized | companion_readonly | db/session, import-state |
| `tests/test_calendar_owner_scope.py` | 344 | 7 | 7 | 0 | security | owner_scope | route/api, db/session, import-state, filesystem, async/threading, ui/static |
| `tests/test_null_owner_gates.py` | 342 | 20 | 20 | 0 | security | owner | route/api, db/session, import-state |
| `tests/test_calendar_recurrence.py` | 338 | 19 | 19 | 0 | services | calendar | - |
| `tests/test_tool_policy.py` | 330 | 13 | 13 | 0 | uncategorized | tool_policy | import-state, async/threading |
| `tests/test_workspace_confine.py` | 328 | 18 | 18 | 0 | uncategorized | workspace_confine | route/api, filesystem, subprocess/script, async/threading |
| `tests/test_diffusion_server_security.py` | 325 | 14 | 14 | 0 | security | security | route/api, import-state, security, filesystem, async/threading, ui/static |
| `tests/test_platform_compat.py` | 317 | 21 | 21 | 0 | uncategorized | platform_compat | import-state, filesystem, subprocess/script |
| `tests/test_upload_routes_owner_scope.py` | 315 | 11 | 11 | 0 | security | owner_scope | route/api, filesystem, async/threading |

## Split planning candidates

This section is generated from metrics, not from manual judgement.
Files are included when they meet at least one threshold:

- at least 300 physical lines; or
- at least 20 collected pytest items.

These are planning candidates only. A later split PR still needs a focused manual review of each file before moving tests.

| File | Why included | Setup/risk signals | Suggested handling |
|---|---|---|---|
| `tests/test_model_routes.py` | 1662 lines, 133 collected tests | route/api, db/session, import-state, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_security_regressions.py` | 1203 lines, 91 collected tests | route/api, db/session, import-state, security, filesystem, async/threading, ui/static | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_provider_classification.py` | 67 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_shell_routes.py` | 460 lines, 62 collected tests | route/api, import-state, filesystem | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_cookbook_helpers.py` | 819 lines, 59 collected tests | route/api, filesystem, subprocess/script, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_pr_blocker_audit.py` | 964 lines, 58 collected tests | import-state, security, filesystem | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_provider_endpoints.py` | 58 collected tests | subprocess/script | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_agent_loop.py` | 458 lines, 51 collected tests | db/session, import-state | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_service_health.py` | 472 lines, 47 collected tests | async/threading | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_run_focus.py` | 399 lines, 47 collected tests | security, filesystem, subprocess/script, ui/static | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_endpoint_probing.py` | 411 lines, 34 collected tests | route/api, db/session, import-state | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_llm_core_anthropic_temp_omit.py` | 32 collected tests | db/session | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_provider_detection.py` | 31 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_model_context.py` | 30 collected tests | db/session, import-state | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_endpoint_resolver.py` | 30 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_embedding_lanes.py` | 1104 lines, 29 collected tests | filesystem | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_upload_limits_centralized.py` | 29 collected tests | import-state, filesystem | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_rename_user_owner_sync.py` | 686 lines, 26 collected tests | route/api, db/session, import-state, filesystem, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_helpers_import_state.py` | 426 lines, 26 collected tests | route/api, db/session, import-state | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_chat_helpers.py` | 26 collected tests | route/api | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_taxonomy.py` | 26 collected tests | security, ui/static | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_llm_core_temperature.py` | 26 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_review_regressions.py` | 902 lines, 25 collected tests | route/api, db/session, import-state, filesystem, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_tool_path_confinement.py` | 24 collected tests | import-state, filesystem, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_copilot.py` | 23 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_research_utils.py` | 23 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_api_chat_security.py` | 401 lines, 22 collected tests | route/api, db/session, import-state, filesystem, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_platform_compat.py` | 317 lines, 21 collected tests | import-state, filesystem, subprocess/script | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_prompt_security.py` | 21 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_null_owner_gates.py` | 342 lines, 20 collected tests | route/api, db/session, import-state | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_tool_support_heuristic.py` | 20 collected tests | No obvious setup signals from static scan. | Good first manual-review candidate if test themes are cohesive. |
| `tests/test_calendar_recurrence.py` | 338 lines | No obvious setup signals from static scan. | Plan split boundaries before editing. |
| `tests/test_workspace_confine.py` | 328 lines | route/api, filesystem, subprocess/script, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_companion_readonly.py` | 372 lines | db/session, import-state | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_imap_leak_fixes.py` | 404 lines | route/api, db/session, security, filesystem | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_auth_regressions.py` | 375 lines | route/api, db/session, import-state, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_api_token_routes.py` | 504 lines | route/api, db/session, import-state, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_diffusion_server_security.py` | 325 lines | route/api, import-state, security, filesystem, async/threading, ui/static | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_tool_policy.py` | 330 lines | import-state, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_endpoint_owner_scope_followup.py` | 414 lines | route/api, db/session, filesystem | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_upload_routes_owner_scope.py` | 315 lines | route/api, filesystem, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_email_owner_scope.py` | 474 lines | route/api, db/session, filesystem, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_upload_handler_atomicity.py` | 401 lines | filesystem, async/threading | Plan split boundaries before editing. |
| `tests/test_kv_cache_invalidation_2927.py` | 463 lines | route/api, db/session, import-state, async/threading | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_calendar_owner_scope.py` | 344 lines | route/api, db/session, import-state, filesystem, async/threading, ui/static | Defer mechanical split until setup/risk boundaries are mapped. |
| `tests/test_skills_manager_owner_isolation.py` | 306 lines | import-state, filesystem | Defer mechanical split until setup/risk boundaries are mapped. |

## Taxonomy coverage gaps among split candidates

`uncategorized` is a current taxonomy area, not a builder failure.
This plan does not reclassify tests because taxonomy changes should be reviewed separately from oversized-file split planning.

Before using any of these files as a split target, first decide whether the taxonomy should be refined in a separate focused issue/PR.

| File | Lines | Collected tests | Sub-area | Signals | Suggested follow-up |
|---|---:|---:|---|---|---|
| `tests/test_pr_blocker_audit.py` | 964 | 58 | pr_blocker_audit | import-state, security, filesystem | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_agent_loop.py` | 458 | 51 | agent_loop | db/session, import-state | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_service_health.py` | 472 | 47 | service_health | async/threading | Review taxonomy mapping before using as a split target. |
| `tests/test_run_focus.py` | 399 | 47 | run_focus | security, filesystem, subprocess/script, ui/static | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_endpoint_probing.py` | 411 | 34 | endpoint_probing | route/api, db/session, import-state | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_model_context.py` | 251 | 30 | model_context | db/session, import-state | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_endpoint_resolver.py` | 148 | 30 | endpoint_resolver | - | Review taxonomy mapping before using as a split target. |
| `tests/test_upload_limits_centralized.py` | 110 | 29 | upload_limits_centralized | import-state, filesystem | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_chat_helpers.py` | 220 | 26 | chat_helpers | route/api | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_taxonomy.py` | 145 | 26 | taxonomy | security, ui/static | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_review_regressions.py` | 902 | 25 | review_regressions | route/api, db/session, import-state, filesystem, async/threading | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_copilot.py` | 170 | 23 | copilot | - | Review taxonomy mapping before using as a split target. |
| `tests/test_platform_compat.py` | 317 | 21 | platform_compat | import-state, filesystem, subprocess/script | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_tool_support_heuristic.py` | 154 | 20 | tool_support_heuristic | - | Review taxonomy mapping before using as a split target. |
| `tests/test_workspace_confine.py` | 328 | 18 | workspace_confine | route/api, filesystem, subprocess/script, async/threading | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_companion_readonly.py` | 372 | 16 | companion_readonly | db/session, import-state | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_imap_leak_fixes.py` | 404 | 15 | imap_leak_fixes | route/api, db/session, security, filesystem | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_tool_policy.py` | 330 | 13 | tool_policy | import-state, async/threading | Review taxonomy and setup/risk boundaries before any split. |
| `tests/test_upload_handler_atomicity.py` | 401 | 9 | upload_handler_atomicity | filesystem, async/threading | Review taxonomy mapping before using as a split target. |
| `tests/test_kv_cache_invalidation_2927.py` | 463 | 8 | kv_cache_invalidation_2927 | route/api, db/session, import-state, async/threading | Review taxonomy and setup/risk boundaries before any split. |

## Suggested first manual-review candidates

These are not automatic split approvals. They are categorized candidates with enough size/collection value and no route/API, DB/session, import-state, or security signal from the static scan.

Files still in the `uncategorized` taxonomy area are listed separately below so taxonomy review does not get mixed into the first split decision.

| File | Lines | Collected tests | Area | Sub-area | Signals | Why this is a candidate |
|---|---:|---:|---|---|---|---|
| `tests/test_provider_classification.py` | 188 | 67 | services | provider | - | 67 collected tests |
| `tests/test_provider_endpoints.py` | 241 | 58 | services | provider | subprocess/script | 58 collected tests |
| `tests/test_provider_detection.py` | 146 | 31 | services | provider | - | 31 collected tests |
| `tests/test_embedding_lanes.py` | 1104 | 29 | services | embedding | filesystem | 1104 lines, 29 collected tests |
| `tests/test_llm_core_temperature.py` | 127 | 26 | services | llm | - | 26 collected tests |
| `tests/test_research_utils.py` | 97 | 23 | services | research | - | 23 collected tests |
| `tests/test_prompt_security.py` | 203 | 21 | security | security | - | 21 collected tests |
| `tests/test_calendar_recurrence.py` | 338 | 19 | services | calendar | - | 338 lines |

## High-risk candidates to defer first

These files may still be split later, but not as the first implementation slice without a separate manual boundary review.

| File | Lines | Collected tests | High-risk signals |
|---|---:|---:|---|
| `tests/test_model_routes.py` | 1662 | 133 | db/session, import-state, route/api |
| `tests/test_security_regressions.py` | 1203 | 91 | db/session, import-state, route/api, security |
| `tests/test_shell_routes.py` | 460 | 62 | import-state, route/api |
| `tests/test_cookbook_helpers.py` | 819 | 59 | route/api |
| `tests/test_pr_blocker_audit.py` | 964 | 58 | import-state, security |
| `tests/test_agent_loop.py` | 458 | 51 | db/session, import-state |
| `tests/test_run_focus.py` | 399 | 47 | security |
| `tests/test_endpoint_probing.py` | 411 | 34 | db/session, import-state, route/api |
| `tests/test_llm_core_anthropic_temp_omit.py` | 94 | 32 | db/session |
| `tests/test_model_context.py` | 251 | 30 | db/session, import-state |
| `tests/test_upload_limits_centralized.py` | 110 | 29 | import-state |
| `tests/test_rename_user_owner_sync.py` | 686 | 26 | db/session, import-state, route/api |
| `tests/test_helpers_import_state.py` | 426 | 26 | db/session, import-state, route/api |
| `tests/test_chat_helpers.py` | 220 | 26 | route/api |
| `tests/test_taxonomy.py` | 145 | 26 | security |

## Rules for future split PRs

- One file or one coherent file-family per PR.
- No assertion rewrites mixed with file moves.
- No helper extraction mixed with file moves.
- No production code changes.
- No CI workflow changes.
- Preserve existing markers and taxonomy unless the split issue explicitly says otherwise.
- Validate the original file's collected tests before and after the split.
- Validate any neighboring taxonomy/focused-runner behavior if paths change.
- Treat files with route/API, DB/session, import-state, or security signals as higher-risk until manually reviewed.

## Suggested next step

Use this plan to choose the first actual oversized-file split issue.
The first split should prefer a file with high review value and low setup risk.
Do not start a split PR from this planning issue alone if the file's boundaries are still ambiguous.

## Reproduction command

This document was generated with:

```bash
.venv/bin/python tests/tools/build_oversized_test_split_plan.py
```

## Freshness check

After editing the builder or rebasing the branch, regenerate the plan and confirm no unexpected plan drift:

```bash
.venv/bin/python tests/tools/build_oversized_test_split_plan.py
git diff --exit-code -- tests/OVERSIZED_TEST_SPLIT_PLAN.md
```
