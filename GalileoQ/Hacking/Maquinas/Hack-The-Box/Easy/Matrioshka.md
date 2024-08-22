#Easy #Linux #tecnicas 
# [Machine Name]

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#machine-name)

## Introduction

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#introduction)

[Include why you made this box, what skills and vulnerabilities you wanted to highlight, etc]

## Info for HTB

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#info-for-htb)

### Access

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#access)

Passwords:

|User|Password|
|---|---|
|user1|[pass phrase, not too hard to type]|
|user2|[pass phrase, not too hard to type]|
|root|[pass phrase, not too hard to type]|

### Key Processes

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#key-processes)

[Describe processes that are running to provide basic services on the box, such as web server, FTP, etc. **For any custom binaries, include the source code (in a separate file unless very short)**. Also, include if any of the services or programs are running intentionally vulnerable versions.]

### Automation / Crons

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#automation--crons)

[Describe any automation on the box:

- What does it do?
- Why? (necessary for exploit step, clean up, etc)
- How does it do it? Provide source code (anything longer than a few lines in a separate attachment)
- How does it run?

]

### Firewall Rules

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#firewall-rules)

[Describe any non-default firewall rules here]

### Docker

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#docker)

[Describe how docker is used if at all. Attach Dockerfiles]

### Other

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#other)

[Include any other design decisions you made that the HTB staff should know about]

# Writeup

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#writeup)

[

Provide an in-depth explanation of the steps it takes to complete the box from start to finish. Divide your walkthrough into the below sections and sub-sections and include images to guide the user through the exploitation.

Please also include screenshots of any visual elements (like websites) that are part of the submission. Our review team is not only evaluating the technical path, but the realism and story of the box.

Show **all** specific commands using markdown's triple-backticks (` ```bash `) such that the reader can copy/paste them, and also show the commands' output through images or markdown code blocks (` ``` `).

**A reader should be able to solve the box entirely by copying and pasting the commands you provide.**

]

# Enumeration

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#enumeration)

[Describe the steps that describe the box's enumeration. Typically, this includes a sub-heading for the Nmap scan, HTTP/web enumeration, etc.]

# Foothold

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#foothold)

[Describe the steps for obtaining an initial foothold (shell/command execution) on the target.]

# Lateral Movement (optional)

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#lateral-movement-optional)

[Describe the steps for lateral movement. This can include Docker breakouts / escape-to-host, etc.]

# Privilege Escalation

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#privilege-escalation)

[Describe the steps to obtaining root/administrator privileges on the box.]