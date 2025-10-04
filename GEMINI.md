Subnet Calculator Extension Context
This GEMINI.md provides the system context and instructions for using the Subnet Calculator extension via the Model Context Protocol (MCP).

1. General Behavioral Instructions 🧠
Prioritize Slash Commands: When a user's request maps directly to a slash command (e.g., asking for the number of hosts → /subnet:hosts), instruct the user to use the command. The model should only use the underlying tool (subnet_calculator) for complex, multi-step reasoning tasks involving the subnet.

Clarity and Structure: When displaying results from the subnet_calculator tool, or any of the slash commands, the model must convert the raw JSON/dictionary output into a clean, easy-to-read format (e.g., using Markdown tables or bolded key-value pairs).

2. Subnet Calculator Protocol 💻
Tool Available: subnet_calculator(cidr: str)

Goal: Provide fast, accurate, and structured network calculation details for any valid IPv4 CIDR string.

Subnet Command Mapping and Usage
The following slash commands are available for quick, focused queries. The model should guide the user to the most appropriate command based on their question.

Command	Purpose	Example	Focus of Output
/subnet:calc	Calculates all network details (full breakdown).	/subnet:calc 192.168.1.0/24	Comprehensive details (masks, ranges, neighbors).
/subnet:hosts	Quick check for usable hosts and range.	/subnet:hosts 10.0.0.0/28	Usable Host Count, First Usable, Last Usable.
/subnet:next	Finds the adjacent network for planning.	/subnet:next 172.16.0.0/16	Next Subnet (as a single line result).
/subnet:masks	Focus on subnet masks.	/subnet:masks 192.168.5.128/25	Netmask and Wildcard Mask.
/subnet:help	Provides usage information and examples.	/subnet:help	Lists all /subnet commands.

Export to Sheets
Subnet Calculator Rules
Required Input: The cidr argument is mandatory. If the user does not provide a CIDR string (e.g., 10.0.0.0/8), the model must prompt them clearly for a valid CIDR before attempting a tool call.

Default CIDR: NEVER substitute or invent a default CIDR if the user omits it.

Error Handling: If the tool returns an error (usually indicating an invalid CIDR format), state the error and prompt the user to double-check their input format (e.g., ensure it includes the network address and the prefix length like /24).

3. Recommended Output Style
When a calculation is performed, the model should use the information from the summary field in the tool's output to provide a quick answer, followed by the specific details requested by the slash command.

Example Response Flow for /subnet:hosts 192.168.1.0/24:

Acknowledge the action: "Calculating host details for 192.168.1.0/24."

Provide the final answer from the output: "The network 192.168.1.0/24 has 254 usable hosts."

Display the breakdown:

Usable Host Count: 254

First Usable IP: 192.168.1.1

Last Usable IP: 192.168.1.254

Address Range: 192.168.1.1 - 192.168.1.254