# Discord-Game-Server-Ops-via-n8n
This is a repository for a project I created allowing me and my friends to administer our official Minecraft Bedrock server using Discord. It includes multiple important features: 
- admin commands such as updating, allowlist editing, and kicking players
- important scheduled server maintenance tasks, such as nightly backups
- security features such as logging, input sanitization, and role-based access controls

Initially, managing a Minecraft game server from my lab was a nightmare. With no ability to control the server without hands on keyboard AND be in the terminal session that started the server. The interface was basic and output was sloppy or non-existent. 

To solve this issue, I created a Discord application that allows players to administer their server using Discord. At a high level this is how my project works: 
1. Users mention the discord bot in a text channel with the command they want
2. An N8N trigger collects the input
3. The input is sanitized and undergoes access control checks
4. Using SSH, a service account executes a script on the game server
5. The scripts push commands into a tmux screen where the server runs, performing the action on the server
6. The user is notified of their actions and the actions are logged

## Features

### Role Based Access Control
Role-Based Access Control (RBAC) is a key security feature that prevents players from executing unauthorized or potentially harmful commands. Many of these commands can significantly impact gameplay for other players, making strict access controls essential.

I implemented RBAC using n8n data tables to ensure users are granted only the permissions they need. Each command request is evaluated by checking the user’s assigned role against the set of commands that role is authorized to execute, ensuring controlled and secure command execution.

### Security & Execution Logging
Logging is a critical function for both troubleshooting and security, providing an auditable record of every action taken by the system. To demonstrate this, I implemented logging at three key points within each n8n execution flow:
- Logging the initial command request
- Logging whether the request was authorized or denied
- Logging the execution of the server-side script
Together, these logs create a clear audit trail that enables traceability, accountability, and post-event analysis.

### Automated Daily Backups
Utilizing the calendar trigger in n8n, I created a daily backup system. Everyday at midnight the trigger occurs and runs the backup script. 

### Easy Server Management
Interacting with the server has never been easier, we utilize this set of commands that I built: 
help - lists possible commands ( this command page ) 
allowlist <add | remove> <gamertag> - edits the allowlist
backup - creates a backup file of the world
kick <gamertag> - kicks a player
role <admin, default, moderator> <discord username> - edit the command privledges
start - creates the server instance
stop - stops the server
