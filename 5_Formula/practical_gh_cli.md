# Git Commands

## Add, Commit, and Push Changes

```sh
git pull; git add -A ; git commit -m "Thanos Proxy implement " -m "initial commit " ; git push;clear; git status;
```


---
## Automate this in vs code:

1. **Open your `tasks.json` file in VS Code.**
2. **Replace the existing task with the following:**

    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "label": "Git Commands",
                "type": "shell",
                "command": "git pull && git add -A && git commit -m '${input:commitMessage}' && git push && clear && git status",
                "problemMatcher": [],
                "inputs": [
                    {
                        "id": "commitMessage",
                        "type": "promptString",
                        "description": "Enter commit message"
                    }
                ]
            }
        ]
    }
    ```

3. **Save the `tasks.json` file.** Now, when you run the task, it will prompt you to enter a commit message.

4. **Run the task:**
    - **Windows/Linux**: Press `Ctrl+Shift+P`
    - **Mac**: Press `Cmd+Shift+P`

    It will prompt you to enter a commit message.

    ## Additional Tips

    ### Using Git Aliases

    To simplify your Git commands, you can create aliases. Add the following to your `.gitconfig` file:

    ```ini
    [alias]
        acp = "!f() { git add -A && git commit -m \"$1\" && git push; }; f"
    ```

    Now, you can use `git acp "your commit message"` to add, commit, and push changes with a single command.

    ### VS Code Git Integration

    VS Code has built-in Git support. You can perform Git operations directly from the Source Control view:

    1. **Open the Source Control view**: Click on the Source Control icon in the Activity Bar on the side of the window.
    2. **Stage changes**: Click the `+` icon next to the files you want to stage.
    3. **Commit changes**: Enter your commit message in the input box at the top and click the checkmark icon.
    4. **Push changes**: Click the `...` menu at the top of the Source Control view and select `Push`.

    These features can help streamline your workflow without needing to switch to the terminal.