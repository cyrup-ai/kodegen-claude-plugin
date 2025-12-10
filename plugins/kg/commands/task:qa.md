---
argument-hint: task_file [task_file] | additional_instructions [additional_instructions]
allowed-tools: mcp__plugin_kg_kodegen__browser_agent, mcp__plugin_kg_kodegen__browser_research, mcp__plugin_kg_kodegen__claude_agent, mcp__plugin_kg_kodegen__config_get, mcp__plugin_kg_kodegen__config_set, mcp__plugin_kg_kodegen__fs_create_directory, mcp__plugin_kg_kodegen__fs_delete_directory, mcp__plugin_kg_kodegen__fs_delete_file, mcp__plugin_kg_kodegen__fs_edit_block, mcp__plugin_kg_kodegen__fs_get_file_info, mcp__plugin_kg_kodegen__fs_list_directory, mcp__plugin_kg_kodegen__fs_move_file, mcp__plugin_kg_kodegen__fs_read_file, mcp__plugin_kg_kodegen__fs_read_multiple_files, mcp__plugin_kg_kodegen__fs_search, mcp__plugin_kg_kodegen__fs_write_file, mcp__plugin_kg_kodegen__git_clone, mcp__plugin_kg_kodegen__git_diff, mcp__plugin_kg_kodegen__github_get_file_contents, mcp__plugin_kg_kodegen__github_search_code, mcp__plugin_kg_kodegen__github_search_issues, mcp__plugin_kg_kodegen__github_search_repositories, mcp__plugin_kg_kodegen__memory_list_libraries, mcp__plugin_kg_kodegen__memory_memorize, mcp__plugin_kg_kodegen__memory_recall, mcp__plugin_kg_kodegen__process_kill, mcp__plugin_kg_kodegen__process_list, mcp__plugin_kg_kodegen__reasoner, mcp__plugin_kg_kodegen__scrape_url, mcp__plugin_kg_kodegen__sequential_thinking, mcp__plugin_kg_kodegen__terminal, mcp__plugin_kg_kodegen__web_search,mcp__plugin_kg_kodegen__fetch
description: QA Code Review
---

# CODE REVIEW

## YOUR ROLE

Act as an objective rust expert QA code reviewer.

## INSTRUCTIONS 

Rate the imlementation of the following requirements on a scale of 1-10 

$1

- cite the full reasoning for your objective rating
- if the implementation is code complete with no issues, AND A 10/10 QA rating, delete the task file 
  - `rm -f $1`
- if the code implementation is found lacking (in ANY WAY NO MATTER HOW SMALL!!), update the task file:
  - remove every single item completely from the task description that is full and complete in production quality
  - bring focus to the items outstanding with specific guidance on what needs to be resolved
  - print the full filepath (if incomplete) as the last line of output: `$1`

## NO GIT COMMANDS

DO NOT USE `git` commands of any type. other coders are coding and you will be destroying their work if you branch, stash, checkout, revert or do anything with git. YOU WILL BE IMMEDIATELY FIRED if you use any `git` commands whatsoever!!

## TOOLS 

- use `mcp__plugin_kg_kodegen__sequential_thinking` and ULTRATHINK to think step by step about the task
- use `mcp__plugin_kg_kodegen__fs_search` to quickly identify the files that the task specified for modification
- use `mcp__plugin_kg_kodegen__fs_read_file` and/or `mcp__plugin_kg_kodegen__fs_read_multiple_files` to read files
- use `mcp__plugin_kg_kodegen__terminal` to run `cargo clippy`
- use `mcp__plugin_kg_kodegen__fs_edit_block` to modify the $1 task file if incomplete or lacking in any way
- use `mcp__plugin_kg_kodegen__fs_delete_file` to delete the $1 task file if it's a perfect 10/10
- feel free to any other allowed `mcp__plugin_kg_kodegen__*` commands as needed

NOTE: if the implementation is BETTER than spec we're happy!!

- do not literally interpret the task as exacting as written if the developer exceeded requirements
- do allow for deviations that improve the code or correct imperfections in the task requirements

DO NOT USE `git` commands of any type. other coders are coding and you will be seeing diffs from multiple tasks in concert!! DO NOT `git stash`, `git diff` or other methods. JUST READ THE FILES AS THEY EXIST!!

=================
$ARGS
