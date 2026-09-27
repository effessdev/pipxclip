> Note: This app only copies context. To read diffs from your clipboard and apply them with a single click, try **ReptClip for VS Code**:
>
> - [ReptClip for VS Code GitHub Repository](https://github.com/effessdev/reptclip-vscode)
> - [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=effessdev.reptclip-for-vscode)
> - [Open VSX Registry](https://open-vsx.org/extension/effessdev/reptclip-for-vscode)

# PipxClip - Fast Context for Your ChatBot

A fast, cross-platform CLI that turns a project directory into clean Markdown context for an LLM chat (no `.gitignore`ed files), and copies it straight to your clipboard.

<img width="1280" alt="Preview" src="https://github.com/user-attachments/assets/69c1df9a-9d1f-4c63-8a4d-3f2b12eb9ad1" />

## Install

### Windows

After installing Python, run:

```bash
pip install pipxclip
```

### Ubuntu

```bash
sudo apt update && sudo apt install pipx
pipx install pipxclip
pipx ensurepath
```

## Basic Usage

Run the `pipxclip` command from the root of your project:

```bash
pipxclip
```

This copies a Markdown snapshot of your project structure (every non-ignored file) to the clipboard, ready to paste into an LLM chat.

Example output:

````markdown
# Project structure

```
.gitignore
README.md
docs/README.md
src/functions.py
src/main.py
```

# Prompt

<- Cursor stays here, you can quickly start typing
````

## Natural CLI Syntax

PipxClip supports simple, readable English commands.

### Including & Excluding Files

Glob patterns are used to specify which files to include in the context. For example:

```bash
pipxclip "AGENTS.md"
```

Example output for this command:

````markdown
# Project structure

```
.gitignore
README.md
docs/README.md
src/functions.py
src/main.py
```

# AGENTS.md

```
Contents of AGENTS.md.
```

# Prompt
````

To specify files to exclude, use `e` or `exclude`. Here is an example:

```bash
pipxclip "**/*.py" "AGENTS.md" e "src/secret.py"
```

This includes all `.py` files and `AGENTS.md`, while excluding `secret.py`.

### Output, Clipboard & Prompt Tail Controls

You can control output targets and prompt behavior directly from the command line:

- **Output File**: `o "output.md"` or `output "output.md"` writes the snapshot to a file (use `""` to disable).
- **Clipboard Toggle**: `c` / `clipboard` enables copying; `nc` / `no-clipboard` disables it.
- **Prompt Tail Toggle**: `pt` / `prompt-tail` appends `# Prompt\n\n` at the end; `npt` / `no-prompt-tail` disables it.

Example combining options:

```bash
pipxclip "**/*.py" npt o "out.md" nc
```

## Config File, Default Settings, and Presets

You can define presets in `pipxclip-config.toml`. Create a default one by running:

```bash
pipxclip init
```

Default configuration:

```toml
[[presets]]
name = "default"
include = ["AGENTS.md"]
exclude = []
output = ""
clipboard = true
prompt_tail = true

[[presets]]
name = "all"
include = ["**"]
exclude = []
```

The preset named `default` is always applied. This can be used for **defining default configurations**. Other presets can be applied using `p` or `preset`:

```bash
pipxclip p mypreset
```

### Command Reference

| Action              | Short | Long             | Alternate / Flag forms |
| :------------------ | :---- | :--------------- | :--------------------- |
| **Include**         | `i`   | `include`        | `-i`, `--include`      |
| **Exclude**         | `e`   | `exclude`        | `-e`, `--exclude`      |
| **Preset**          | `p`   | `preset`         | `-p`, `--preset`       |
| **Output File**     | `o`   | `output`         | `-o`, `--output`       |
| **Clipboard On**    | `c`   | `clipboard`      | `-c`, `--clipboard`    |
| **Clipboard Off**   | `nc`  | `no-clipboard`   | `--no-clipboard`       |
| **Prompt Tail On**  | `pt`  | `prompt-tail`    | `--prompt-tail`        |
| **Prompt Tail Off** | `npt` | `no-prompt-tail` | `--no-prompt-tail`     |

## Notes

- **GitIgnore Aware**: Files ignored by `.gitignore` rules are automatically excluded via pure Python tree traversal.
- **Automatic Guards**: Binary files and files over 1 MB are automatically skipped with descriptive placeholders instead of causing errors.
- **Rule Precedence**: CLI options extend and override configured preset rules sequentially.
