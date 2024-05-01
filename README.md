# BCP-Grafana

The main purose of this tool is for monitor the synchronization status of two directories, which are on local server and remote server.

Initially, the tool establishes connections to both servers and retrieves the MD5 checksum values of each file within a specified folder. It then conducts an analysis of these checksum values to detect any discrepancies. Subsequently, the tool proceeds to store this analyzed data into a MySQL database for further processing.

The stored data is then utilized to generate comprehensive visualizations through a Grafana Dashboard, facilitating enhanced visibility and insight into the synchronization status.

Additionally, a command-line interface has been developed to facilitate CRUD operations on server pairs and syncing rules, thereby offering streamlined management and configuration capabilities.

## Arechitecture of the Tool

Below is a comprehensive diagram which shows all the components of the tool.

<img src="https://github.com/PasinduBhagya/BCP-Grafana/assets/63937160/4e2a7c8f-9137-4e29-9b7d-8e68d2db936a" width="600">

1. **Local Server and BCP Server:**
    - Local Server: Responsible for initiating data synchronization to the Backup (BCP) Server.
    - BCP Server: Acts as the destination for synchronized data.
2. **Application Server:**
    - Data Gathering Tool: Collects MD5 checksums of files within specified directories from both the local and remote servers
    - Data Analyzing Tool: Analyzes collected checksums to identify discrepancies between the directories.
    - CLI (Command Line Interface): Offers a user-friendly command-line interface for managing syncing rules and server pairs.
3. **MySQL Database:** Stores syncing rules, server pair information, and synchronization status data.
4. **Grafana Dashboard:** Visualizes stored data, providing users with intuitive insights into synchronization status and any detected discrepancies.


## Grafana Dashboard Overview

## Monitoring configurations with CLI

To access the command-line interface (CLI) of the tool, utilize the command "bcpsyn" along with the necessary flags. To view a comprehensive list of available commands, execute the following: `bcpsyn --help`. This command will provide a detailed overview of all available flags and their respective functionalities within the tool.
>`--help` - Displays the help menu.

> `--list-projects` - Lists all available projects along with their details.

> `--list-rules` - Displays a comprehensive list of rules associated with the projects.

> `--list-servers` - Shows all registered Local BCP server pairs.

> `--add-rule` - Adds a new rule to specify synchronization criteria.

> `--add-new-server` - Registers a new Local BCP server pair.

> `--remove-rule-with-id` - Removes a specific rule identified by its unique ID.

> `--remove-server` - Deletes a Local BCP server pair (will remove all rules associated with that pair).

> `--run-rules` - Manually runs the rules.

### 1. Add a New Server Pair.
To add a new server pair, please enter the following command: `bcpsyn --add-new-servers` and press enter. Subsequently, the tool will prompt you to input the necessary information for the new server pair.

@ Image here

Upon entering the server username and password, you'll be prompted for the password of the user associated with the specified IP Address. After providing the password, the tool will copy its public key to the authorized keys file of that user. This action enables key-based authentication for future connections to the server.

> [!IMPORTANT]
> If the tool encounters an issue while attempting to copy the public key during this phase, you will receive a prompt to manually add the public key as directed by the tool.

> [!TIP]
> It's advisable to verify whether key-based authentication is enabled by using the command "ssh <username>@<IP Address>"

To confirm the addition of the server pair, you can use the command `bcpsyn --list-servers`. This command will display a list of all available servers.

### 2. To Remove a Server Pair.
To remove a server, input the command bcpsyn `--remove-server` and press enter. You will then be prompted to provide the serversID of the server you wish to remove.

> [!WARNING]
> Removing a server pair will result in the removal of all associated rules.

### 3. Add a New Rule.
To add a new rule, enter the command bcpsyn `--add-rule` and press enter. Subsequently, the tool will prompt you to provide the necessary information.

> [!TIP]
> The extension prompt is optional. You can specify particular extensions (e.g., zip, txt) if needed. To check all files, simply leave the prompt empty.

### 4. List all Rules

To list all rules use `bcpsyn --list-rules` command and press enter. 

### 5. Remove a rule.
To remove a rule enter `bcpsyn --remove-rule` command. Then provide the Rule ID that you need to remove and press enter. Then it will remove that rule.



## How the tool works?
The tools use rules to check the synchronization status of the to folder reside on two diffrent servers. You can list down all the rules by using `bcpsyn --list-rules` command.

Column Name  | Description
------------- | -------------
ID  | Rule ID which will be created automaticaly.
Project Name  | Name of the Project.
Local Server Path  | Absoulte path to the directory on the local server. 
BCP Server Path  | Absoulte path to the directory on the BCP server. 
serversID | The ID of the Servers pair which will be added using `bcpsyn --add-new-server`.
Alias  | The name that will be visible on the Grafana Dashboard.

Each rule has an ServerID which will consits of data of the Local and BCP Server. You can list the server information by using `bcpsyn --list-servers`

