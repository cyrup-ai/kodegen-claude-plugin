---
allowed-tools: mcp__plugin_kg_kodegen__browser_agent, mcp__plugin_kg_kodegen__browser_research, mcp__plugin_kg_kodegen__claude_agent, mcp__plugin_kg_kodegen__config_get, mcp__plugin_kg_kodegen__config_set, mcp__plugin_kg_kodegen__fs_create_directory, mcp__plugin_kg_kodegen__fs_delete_directory, mcp__plugin_kg_kodegen__fs_delete_file, mcp__plugin_kg_kodegen__fs_edit_block, mcp__plugin_kg_kodegen__fs_get_file_info, mcp__plugin_kg_kodegen__fs_list_directory, mcp__plugin_kg_kodegen__fs_move_file, mcp__plugin_kg_kodegen__fs_read_file, mcp__plugin_kg_kodegen__fs_read_multiple_files, mcp__plugin_kg_kodegen__fs_search, mcp__plugin_kg_kodegen__fs_write_file, mcp__plugin_kg_kodegen__git_clone, mcp__plugin_kg_kodegen__git_diff, mcp__plugin_kg_kodegen__github_get_file_contents, mcp__plugin_kg_kodegen__github_search_code, mcp__plugin_kg_kodegen__github_search_issues, mcp__plugin_kg_kodegen__github_search_repositories, mcp__plugin_kg_kodegen__memory_list_libraries, mcp__plugin_kg_kodegen__memory_memorize, mcp__plugin_kg_kodegen__memory_recall, mcp__plugin_kg_kodegen__process_kill, mcp__plugin_kg_kodegen__process_list, mcp__plugin_kg_kodegen__reasoner, mcp__plugin_kg_kodegen__scrape_url, mcp__plugin_kg_kodegen__sequential_thinking, mcp__plugin_kg_kodegen__terminal, mcp__plugin_kg_kodegen__web_search
description: Read the Code!
---

# READ

read FULL FILES with `mcp__plugin_kg_kodegen__sequential_thinking` and KNOW how it works because you READ THE CODE. do not use conjecture to solve this problem. base your recommendations and planning on CERTAINTY and definitive, deterministic INFORMATION based on having READ and COMPREHENDED the context and based your plan on INFORMATION.

If third party libraries are essential to the solution:

- see if we already have sources in `./tmp` (project relative)
- see if we already have sources in `./docs` (project relative)
  - IF NOT use `mcp__plugin_kg_kodegen__git_clone` to clone them into `./tmp/` (project relative) and explore the docs, examples and sources with sequential thinking.

## TOOLS 

- use `mcp__plugin_kg_kodegen__sequential_thinking` and ULTRATHINK to think step by step about the task
- use `mcp__plugin_kg_kodegen__browser_web_search` if research on the web is needed for the task scope
- use `mcp__plugin_kg_kodegen__scrape_url` if you find websites that are key to understanding the task to scrape the full website
  - once completed use `mcp__plugin_kg_kodegen__scrape_search_results` to quickly find the most relevant information from the crawl results
- use `mcp__plugin_kg_kodegen__fs_search` to search the local codebase and understand the architecture and relevant files that may need to be modified or built around
- use `mcp__plugin_kg_kodegen__fs_read_file` and/or `mcp__plugin_kg_kodegen__fs_read_multiple_files` to read files
- use `mcp__plugin_kg_kodegen__git_clone` to clone repositories into `./tmp` for research
- use these tools to explore github directly:
  - `mcp__plugin_kg_kodegen__github_get_file_contents`
  - `mcp__plugin_kg_kodegen__github_search_code`
  - `mcp__plugin_kg_kodegen__github_search_repositories`
  - `mcp__plugin_kg_kodegen__github_search_issues`
- feel free to any other allowed `mcp__plugin_kg_kodegen__*` commands as needed

=================
==> KEY INSIGHT: be _CERTAIN_ BEFORE you code <==
$ARGS
