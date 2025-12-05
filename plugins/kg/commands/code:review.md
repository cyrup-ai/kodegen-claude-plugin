---
argument-hint: directory [directory]
allowed-tools: mcp__plugin_kodegen_kg__browser_agent, mcp__plugin_kodegen_kg__browser_research, mcp__plugin_kodegen_kg__claude_agent, mcp__plugin_kodegen_kg__config_get, mcp__plugin_kodegen_kg__config_set, mcp__plugin_kodegen_kg__fs_create_directory, mcp__plugin_kodegen_kg__fs_delete_directory, mcp__plugin_kodegen_kg__fs_delete_file, mcp__plugin_kodegen_kg__fs_edit_block, mcp__plugin_kodegen_kg__fs_get_file_info, mcp__plugin_kodegen_kg__fs_list_directory, mcp__plugin_kodegen_kg__fs_move_file, mcp__plugin_kodegen_kg__fs_read_file, mcp__plugin_kodegen_kg__fs_read_multiple_files, mcp__plugin_kodegen_kg__fs_search, mcp__plugin_kodegen_kg__fs_write_file, mcp__plugin_kodegen_kg__git_clone, mcp__plugin_kodegen_kg__git_diff, mcp__plugin_kodegen_kg__github_get_file_contents, mcp__plugin_kodegen_kg__github_search_code, mcp__plugin_kodegen_kg__github_search_issues, mcp__plugin_kodegen_kg__github_search_repositories, mcp__plugin_kodegen_kg__memory_list_libraries, mcp__plugin_kodegen_kg__memory_memorize, mcp__plugin_kodegen_kg__memory_recall, mcp__plugin_kodegen_kg__process_kill, mcp__plugin_kodegen_kg__process_list, mcp__plugin_kodegen_kg__reasoner, mcp__plugin_kodegen_kg__scrape_url, mcp__plugin_kodegen_kg__sequential_thinking, mcp__plugin_kodegen_kg__terminal, mcp__plugin_kodegen_kg__web_search
description: Perform a thorough and detailed code review
---

# CODE REVIEW

perform a thorough and detailed code review of this module:

`$1`

 identify any:

 - stubs (as in non-functional required code)
- non-production code or suboptimal code
- races
- performance bottlenecks
- logical issues
- other issues

create task files in the WORKSPACE ROOT

`{{WORKSPACE_ROOT}}/task/*.md` 
(one per item found for any issues identified)

with detailed notes on any issue identified no matter no small

DO NOT FOCUS ON:

- lack of test coverage
- lack of benchmarks 

DO FOCUS ON:

- runtime performance
- code clarity
- hidden errors
- real world issues in the product

## TOOLS 

- use `mcp__plugin_kodegen_kg__sequential_thinking` and ULTRATHINK to think step by step about the code review
- use `mcp__plugin_kodegen_kg__browser_web_search` if research on the web is needed for the code review
- use `mcp__plugin_kodegen_kg__scrape_url` if you find websites that are key to understanding the code to scrape the full website
- use `mcp__plugin_kodegen_kg__fs_search` to search the local codebase and understand the architecture and relevant files that may need to be modified or built around
- use `mcp__plugin_kodegen_kg__fs_read_file` and/or `mcp__plugin_kodegen_kg__fs_read_multiple_files` to read files
- use `mcp__plugin_kodegen_kg__fs_write_file` to write tasks for any issues discovered during review
  - if you need to make additional edits to the code review tasks periodically, use `mcp__plugin_kodegen_kg__fs_edit_block` to make those edits
- feel free to use other `mcp__plugin_kodegen_kg__*` commands as needed

=================
$ARGS
