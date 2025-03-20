---
title: "TryHackMe | Windows PowerShell | RSCyberTech"
date: 2025-01-03
categories: 
  - "general"
---

➡️ **_By @RSCyberTech_**

- _Website: RSCyberTech.com_
- _LinkedIn: linkedin.com/in/ricardoams_

Platform: TryHackMe

Learning Path: Cyber Security 101

Room: Windows PowerShell

# 1️⃣ Task 1 - Introduction

- no answer needed

# 2️⃣ Task 2 - What Is PowerShell

## What do we call the advanced approach used to develop PowerShell?

### Answer ✅

- `object-oriented`

### Workflow / Justification / Source / Steps / Reasoning

- _“… Snover’s solution was to develop an object-oriented approach…”_
- Mentioned in the section’s text.

# 3️⃣ Task 3 - PowerShell Basics

## How would you retrieve a list of commands that **start with** the verb `Remove`? \[for the sake of this question, avoid the use of quotes (" or ') in your answer\]

### Answer ✅

- `Get-Command -Name Remove*`

### Workflow / Justification / Source / Steps / Reasoning

- _“To list all available cmdlets, functions, aliases, and scripts that can be executed in the current PowerShell session, we can use `Get-Command`. It’s an essential tool for discovering what commands one can use. … or each `CommandInfo` object retrieved by the cmdlet, some essential information (properties) is displayed on the console. It’s possible to filter the list of commands based on displayed property values. For example, if we want to display only the available commands of type “function”, we can use `-CommandType "Function"`"_
- Mentioned in the section’s text.
- check help page of the `Get-Command` for more info, like the `-name` parameter

## What cmdlet has its traditional counterpart `echo` as an alias?

### Answer ✅

- `Write-Output`

### Workflow / Justification / Source / Steps / Reasoning

\- 

```
PS C:Userscaptain> get-help *alias*              
Name                              Category  Module                    Synopsis
---- -------- ------ --------
Export-Alias                      Cmdlet    Microsoft.PowerShell.U... Exports information about currently defined aliases to a file.
Get-Alias                         Cmdlet    Microsoft.PowerShell.U... Gets the aliases for the current session.
Import-Alias                      Cmdlet    Microsoft.PowerShell.U... Imports an alias list from a file.
New-Alias                         Cmdlet    Microsoft.PowerShell.U... Creates a new alias.
Set-Alias                         Cmdlet    Microsoft.PowerShell.U... Creates or changes an alias for a cmdlet or other command in the current PowerShell session.
about_Aliases                     HelpFile
about_Alias_Provider              HelpFile
```

\- 

```
PS C:Userscaptain> get-alias -Name echo
CommandType     Name                                               Version    Source
----------- ---- ------- ------
Alias           echo -> Write-Output
```

## What is the command to retrieve some example usage for the cmdlet `New-LocalUser`?

### Answer ✅

- `Get-Help New-LocalUser -examples`

### Workflow / Justification / Source / Steps / Reasoning

- _“Another essential cmdlet to keep in our tool belt is `Get-Help`: it provides detailed information about cmdlets, including usage, parameters, and examples. It’s the go-to cmdlet for learning how to use PowerShell commands.”_
- Mentioned in the section’s text.

# 4️⃣ Task 4 - Navigating the File System and Working with Files

## What cmdlet can you use instead of the traditional Windows command `type`?

### Answer ✅

- `Get-Content`

### Workflow / Justification / Source / Steps / Reasoning

- _“Finally, to read and display the contents of a file, we can use the `Get-Content` cmdlet, which works similarly to the `type` command in Command Prompt (or `cat` in Unix-like systems).”_
- Present in the text

## What PowerShell command would you use to display the content of the "C:Users" directory? \[for the sake of this question, avoid the use of quotes (" or ') in your answer\]

### Answer ✅

- `Get-ChildItem`

### Workflow / Justification / Source / Steps / Reasoning

- _”…`Get-ChildItem` lists the files and directories in a location specified with the `-Path` parameter. It can be used to explore directories and view their contents…”_
- Present in the text

## How many items are displayed by the command described in the previous question?

### Answer ✅

- `4`

### Workflow / Justification / Source / Steps / Reasoning

\- 

```
PS C:Userscaptain> ( Get-ChildItem -Path C:Users).count
4
```

# 5️⃣ Task 5 - Piping, Filtering, and Sorting Data

## How would you retrieve the items in the current directory with size greater than 100? \[for the sake of this question, avoid the use of quotes (" or ') in your answer\]

### Answer ✅

- `Get-ChildItem | Where-Object -Property Length -gt 100`

### Workflow / Justification / Source / Steps / Reasoning

- Crafted from the examples in the text

# 6️⃣ Task 6 - System and Network Information

## Other than your current user and the default "Administrator" account, what other user is enabled on the target machine?

### Answer ✅

- `p1r4t3`

### Workflow / Justification / Source / Steps / Reasoning

\- 

````
```
PS C:Userscaptain> Get-LocalUser
Name               Enabled Description
---- ------- -----------
Administrator      True    Built-in account for administering the computer/domain
captain            True    The beloved captain of this pirate ship.
DefaultAccount     False   A user account managed by the system.
Guest              False   Built-in account for guest access to the computer/domain
p1r4t3             True    A merry life and a short one.
WDAGUtilityAccount False   A user account managed and used by the system for Windows Defender Application Guard scenarios.
```
````

## This lad has hidden his account among the others with no regard for our beloved captain! What is the motto he has so bluntly put as his account's description?

### Answer ✅

- `A merry life and a short one.`

### Workflow / Justification / Source / Steps / Reasoning

\- 

```
PS C:Userscaptain> Get-LocalUser
Name               Enabled Description
---- ------- -----------
Administrator      True    Built-in account for administering the computer/domain
captain            True    The beloved captain of this pirate ship.
DefaultAccount     False   A user account managed by the system.
Guest              False   Built-in account for guest access to the computer/domain
p1r4t3             True    A merry life and a short one.
WDAGUtilityAccount False   A user account managed and used by the system for Windows Defender Application Guard scenarios.
```

## Now a small challenge to put it all together. This shady lad that we just found hidden among the local users has his own home folder in the "C:Users" directory. Can you navigate the filesystem and find the hidden treasure inside this pirate's home?

### Answer ✅

- `THM{p34rlInAsh3ll}`

### Workflow / Justification / Source / Steps / Reasoning

\- 

```
PS C:Userscaptain> cat ..p1r4t3hidden-treasure-chestbig-treasure.txt
            ___
        .-"; ! ;"-.
      .'!  : | :  !`.
     /  ! : ! : !  /
    / |  ! :|: !  | /
   (    ; :!: ; / /  )
  ( `.  | !:|:! | / .' )
  (`.   !:|:!/ / / .')
    `.`. |!|! |/,'.' /
    `._`.\!!!// .'_.'
       `.`.\|//.'.'
        |`._`n'_.'|  hjw
        "----^----"
FLAG: THM{p34rlInAsh3ll}
```

# 7️⃣ Task 7 - Real-Time System Analysis

## In the previous task, you found a marvellous treasure carefully hidden in the target machine. What is the hash of the file that contains it?

### Answer ✅

- `71FC5EC11C2497A32F8F08E61399687D90ABE6E204D2964DF589543A613F3E08`

### Workflow / Justification / Source / Steps / Reasoning

```
PS C:Userscaptain> get-filehash ..p1r4t3hidden-treasure-chestbig-treasure.txt   
Algorithm       Hash                                                                   Path
--------- ---- ----
SHA256          71FC5EC11C2497A32F8F08E61399687D90ABE6E204D2964DF589543A613F3E08       C:Usersp1r4t3hidden-treasure-chestbig-treasure.txt

```

## What property retrieved by default by `Get-NetTCPConnection` contains information about the process that has started the connection?

### Answer ✅

- `OwningProcess`

### Workflow / Justification / Source / Steps / Reasoning

\- 

```
PS C:Userscaptain> Get-NetTCPConnection | get-member | sort name | where {$_.name -imatch "process"}   
   TypeName: Microsoft.Management.Infrastructure.CimInstance#ROOT/StandardCimv2/MSFT_NetTCPConnection
Name          MemberType Definition
---- ---------- ----------
OwningProcess Property   uint32 OwningProcess {get;}
```

## It's time for another small challenge. Some vital service has been installed on this pirate ship to guarantee that the captain can always navigate safely. But something isn't working as expected, and the captain wonders why. Investigating, they find out the truth, at last: the service has been tampered with! The shady lad from before has modified the service `DisplayName` to reflect his very own motto, the same that he put in his user description. With this information and the PowerShell knowledge you have built so far, can you find the service name?

### Answer ✅

- `p1r4t3-s-compass`

### Workflow / Justification / Source / Steps / Reasoning

\- 

```
PS C:Userscaptain> Get-Service | sort name | where {$_.DisplayName -imatch "A merry life and a short one"}
Status   Name               DisplayName
------ ---- -----------
Running  p1r4t3-s-compass   A merry life and a short one.
```

# 8️⃣ Task 8 - Scripting

## What is the syntax to execute the command `Get-Service` on a remote computer named "RoyalFortune"? Assume you don't need to provide credentials to establish the connection. \[for the sake of this question, avoid the use of quotes (" or ') in your answer\]

### Answer ✅

- `Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }`

### Workflow / Justification / Source / Steps / Reasoning

- crafted from the text examples

# 9️⃣ Task 9 - Conclusion

- no answer needed

➡️ **_By @RSCyberTech_**

- _Website: RSCyberTech.com_
- _LinkedIn: linkedin.com/in/ricardoams_

Go to Source
