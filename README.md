# NuDo (Nushell toDo)

<div align="center"><img src="cover.jpg"></div>

A simple, self-contained Todo/Task CLI tool built for [Nushell].

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
    - [Projects](#projects)
    - [TODOs](#todos)
    - [Tasks](#tasks)
- [Configuration](#configuration)
- [File Locations](#file-locations)
- [Dependencies](#dependencies)
- [License](#license)

## Introduction

NuDo is a lightweight task management tool written entirely in Nushell script.
It allows you to manage project-specific TODO lists and standalone tasks with
deadlines directly from your terminal. It leverages Nushell's built-in SQLite
integration for data persistence.

## Features

- **Project-based**: Organize TODOs into custom groups (e.g., backlog, todo, wip, done).
- **Standalone Tasks**: Manage individual tasks with metadata (title, expiration date, completion status).
- **Self-Contained**: Everything runs from a single script and stores data in a local SQLite database.
- **Cross-Platform**: Supports Linux and Windows (sorry I have no MacOS to test it).
- **Integrated Editing**: Uses your system's `$EDITOR`/`$VISUAL` for creating and editing tasks.
- **Beautiful Output**: Color-coded terminal output with optional support for [glow] or [bat] for task viewing.

## Installation

1. Ensure you have [Nushell] installed.
2. Download the `nudo` script or clone it (it is also part of my [DoNuT] glazes!).
3. Make it executable: `chmod u+x nudo`
4. (Optional) Move it to your path: `$env.PATH = [/path/to/your/script] ++ $env.PATH`

## Usage

Run `nudo --help` to see the available commands.

### Projects

Projects allow you to group TODOs. You can set a "default" project to avoid
specifying it every time.

| ACTION              | COMMAND                                                              | NOTES                                                       |
| ------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------- |
| Create a project    | `nudo projects new "My Project" --columns "1:backlog,2:todo,3:done"` |
| Delete project      | `nudo projects delete "My Project"`                                  | If the project name is omitted, a selection list will open. |
| Get default project | `nudo projects get-default`                                          |
| List projects       | `nudo projects list`                                                 |
| Rename project      | `nudo projects rename "Old Name" "New Name"`                         |
| Set default project | `nudo projects set-default "My Project"`                             | If the project name is omitted, a selection list will open. |

Whenever more than one project with the same name is found, a selection list is
provided to the user.

### TODOs

TODOs are items within a project's columns.

| ACTION      | COMMAND                                 | NOTES                                       |
| ----------- | --------------------------------------- | ------------------------------------------- |
| Add a TODO  | `nudo todos new "todo" "My first task"` | If title is omitted, your editor will open. |
| Delete TODO | `nudo todos delete`                     | Opens selection list.                       |
| Edit TODO   | `nudo todos edit`                       | Opens selection list and then editor.       |
| List TODOs  | `nudo todos list`                       |
| Move TODO   | `nudo todos move "todo" "done"`         | Opens selection list.                       |

### Tasks

Tasks are standalone items that include a Markdown-like header for metadata.

| ACTION      | COMMAND             | NOTES                             |
| ----------- | ------------------- | --------------------------------- |
| Create task | `nudo tasks new`    | Opens the editor with a template. |
| Delete task | `nudo tasks delete` |                                   |
| Edit task   | `nudo tasks edit`   |                                   |
| List tasks  | `nudo tasks list`   |                                   |
| View task   | `nudo tasks view`   |                                   |

The default header is as follows:

```md
---
title: task title
expires: <date> <time>
completed: true
---
```

Only the `title` field is mandatory. The `expires` field adds a label to the list.
If the `completed` field is present and set to `true`, the task will be marked
as done. From that point on, it cannot be edited—only deleted.

## Configuration

NuDo generates a configuration file at first run. You can customize colors and
default columns in `nudo.toml`.

```toml
[palette]
default = { fg = '#bbbbbb' }
error = { fg = '#bf3000' }
item = { fg = '#ffffff' }
success = { fg = '#00bf00' }
warning = { fg = '#bf9000' }

[columns]
default = 'backlog,2:todo,3:wip,4:done'

[columns.palette]
backlog = { fg = '#111111', bg = '#888888' }
done = { fg = '#111111', bg = '#00bf3f' }
todo = { fg = '#111111', bg = '#00bfcf' }
wip = { fg = '#111111', bg = '#efbf00' }
```

The format to describe each column is `position:column_name`; if `position` is
omitted, it defaults to position 1

Under the `[columns.palette]` section, it is possible to define the terms used
to colorize TODO groups. They are matched using the `=~` (`LIKE`) operator,
meaning that as long as the term is present in the group name, it will be a match.

## File Locations

- Configuration:
    - Linux: `~/.config/nudo/nudo.toml` (or `$XDG_CONFIG_HOME`)
    - Windows: `%USERPROFILE%\.config\nudo\nudo.toml` (or `%HOMEDRIVE%%HOMEPATH%`)
- Database:
    - Linux: `~/.local/share/nudo/nudo.db` (or `$XDG_DATA_HOME`)
    - Windows: `%LOCALAPPDATA%\nudo\nudo.db` (or `%APPDATA%`)

## Dependencies

- [Nushell] (Required)
- `$EDITOR` or `$VISUAL` environment variable (Required for editing).
- [glow] or [bat] (Optional, for enhanced task viewing).

## License

MIT - Copyright (c) 2026 Marco Trulla

[nushell]: https://www.nushell.sh/ "Nushell"
[glow]: https://github.com/charmbracelet/glow "Glow"
[bat]: https://github.com/sharkdp/bat "Bat"
[donut]: https://github.com/Ragnarokkr/donut "Dotfiles-Nushell Tracker"
