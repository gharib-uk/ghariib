---
title: "FuzPad - A minimalistic note management solution. Powered by fzf"
date: 2025-01-22
---

# 📙FuzPad!

_FuzPad is a minimalistic note management solution. Powered by ⚡junegunn/fzf_⚡

![Sample Image](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fi.imgur.com%2F5WXsOH1.png) ![Sample Image](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fi.imgur.com%2FlkyY8Fe.png) ![Sample Image](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fi.imgur.com%2FNMbPXN7.png) ![Sample Image](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fi.imgur.com%2F5IljHKS.png)

## Features

- **New**: Create a new note with the current date and time as the filename.
- **Open**: Open an existing note.
- **Search**: Search within notes for specific content.
- **Delete**: Delete selected notes.
- **Version control**: Automatically commits changes to your notes.

## Planned Features

- **Tags**: tagging system
- **Make an installer**: use brew for packaging
- **CLI**: create a cli that allows piping, etc.

## Goals

- Provide a straightforward and minimalistic note management solution using a Bash script.
- Ensure ease of use with intuitive commands and interface.

## Requirements

- Bash
- fzf (fuzzy finder)
- bat (for enhanced preview)
- Your preferred text editor (default is `nano`)

## Usage

1. Clone the repository:

```
git clone https://github.com/JianZcar/notes-bash.git
cd notes-bash
```

1. Make the script executable:

```
chmod +x bin/notes
```

1. Run the script:

```
./bin/notes
```

## Configuration

- **Default Directory**: Change the default directory for notes by modifying the `FUZPAD_DIR` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_DIR="$HOME/Documents/.notes"
```

- **Text Editor**: Change the text editor by modifying the `EDITOR` variable in the script or by setting it in your `~/.bashrc` file:

```
  export EDITOR="nano"
```

- **Text Format**: Change the text format by modifying the `TEXT_FORMAT` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_TEXT_FORMAT="txt"
```

- **Date Time Format**: Change the date and time format used for note filenames by modifying the `DATE_TIME_FORMAT` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_DATE_TIME_FORMAT="%Y-%m-%d-%H-%M-%S"
```

- **BAT Theme**: Change the theme used by `bat` for previewing notes by modifying the `BAT_THEME` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_BAT_THEME="OneHalfLight"
```

- **Reverse List**: Set to `true` to reverse the order of the list when opening or deleting notes by modifying the `REVERSE_LIST` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_REVERSE_LIST="false"
```

- **Sort Format**: Change the sorting format for listing notes (`T@` for creation date, `Y` for modified date) by modifying the `SORT_FORMAT` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_SORT_FORMAT="T@"
```

- **Preview Size**: Change the size of the preview window for `fzf` by modifying the `PREVIEW_SIZE` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_PREVIEW_SIZE="70%"
```

- **Start Line Search Preview**: Set the starting line number for the search preview by modifying the `START_LINE_SEARCH_PREVIEW` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_START_LINE_SEARCH_PREVIEW="5"
```

- **End Line Search Preview**: Set the ending line number for the search preview by modifying the `END_LINE_SEARCH_PREVIEW` variable in the script or by setting it in your `~/.bashrc` file:

```
  export FUZPAD_END_LINE_SEARCH_PREVIEW="9999"
```

After adding the necessary variables to your `~/.bashrc` file, remember to source it to apply the changes:  

```
source ~/.bashrc
```

## Contributing

Feel free to fork the repository and submit pull requests. Contributions are welcome.  
Happy notetaking

Go to Source
