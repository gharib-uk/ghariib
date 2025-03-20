---
title: "🌟 How to Fix Node.js Path Issues in VS Code (Step-by-Step Guide)"
date: 2025-01-12
---

Are you facing issues with Node.js commands not working in your VS Code terminal? 🤔 This is a common problem, especially if you use `nvm` (Node Version Manager) to manage multiple versions of Node.js. In this blog, we’ll walk you through a clear, step-by-step process to fix Node.js path issues and ensure a seamless experience in Visual Studio Code. 💻✨

## 🛠️ Step 1: Check Your Current Node.js Path

Start by verifying which Node.js version is currently active. Run this command in your terminal:  

```
which node
```

You might see something like:  

```
/home/jarvis/.nvm/versions/node/v20.9.0/bin/node
```

📌 This indicates your Node.js is installed via `nvm`. However, VS Code might not be using this path. Let’s fix that. 🚀

## ⚙️ Step 2: Update Your Shell Configuration File

To ensure that `nvm` works properly in VS Code, you must add its configuration to your shell startup file. Depending on your shell, open one of the following files:

- For Bash: `~/.bashrc`
- For Zsh: `~/.zshrc`
- For Profile: `~/.profile`

Add the following lines at the end of the file:  

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] &&  . "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] &&  . "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

After saving the file, reload the shell configuration:  
`source ~/.bashrc` Use Bash  
`source ~/.zshrc` if you’re using Zsh  
✅ This ensures your shell knows where to find Node.js.

## 🔄 Step 3: Set a Default Node.js Version

To make sure the correct Node.js version is loaded every time, set a default version in `nvm`:  

```
nvm alias default v20.9.0
```

📌 This ensures that nvm automatically uses Node.js version v20.9.0 in all new terminal sessions. 🏁

## 🔁 Step 4: Restart VS Code

After updating your shell configuration, restart VS Code to apply the changes. Open the integrated terminal and verify your Node.js version:  

```
node -v
```

✨ If everything is set up correctly, you should see the version you configured with `nvm`. 🎉

## 🛡️ Step 5: Verify VS Code’s Terminal Settings

Ensure that the VS Code uses the correct shell for its terminal. Here’s how to check:

1. Open the Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P`).
2. Search for `Preferences: Open Settings (JSON)` and click it.
3. Add or verify the terminal profile settings. For example:

```
 "terminal.integrated.defaultProfile.linux": "bash",
    "terminal.integrated.profiles.linux": {
        "bash": {
            "path": "/bin/bash"
        }
    }
```

You can edit this file via either the nano 📝 or vim 🖥️ editor. To do so, use one of the following commands in your terminal:

1. `nano ~/.config/Code/User/settings.json ✏️`
2. `vim ~/.config/Code/User/settings.json 💻`

## 📝 Step 6: Add Environment Variables in VS Code Settings

If the terminal still doesn’t recognize Node.js, you can explicitly configure environment variables in VS Code:

1. Open Settings (`Ctrl+,` or `Cmd+,`).
2. Search for `terminal.integrated.env.linux`.
3. Add the following configuration:

```
"terminal.integrated.env.linux": {
        "NVM_DIR": "/home/jarvis/.nvm",
        "PATH": "/home/jarvis/.nvm/versions/node/v20.9.0/bin:${env:PATH}"
    }
```

🔑 This ensures that VS Code’s terminal uses the correct `nvm` path. 🛤️

## 🚀 Step 7: Test the Configuration

Open a new terminal in VS Code and test the setup by running:  

```
node -v
```

🎯 You should now see the correct Node.js version displayed. Success! 🎉

## 🎉 Conclusion

By following these steps, you can resolve Node.js path issues in VS Code and create a smooth development environment. 🌟 Whether you’re working on a new project or managing multiple Node.js versions, this guide ensures that your tools are properly configured. Happy coding! 💻💡

Go to Source
