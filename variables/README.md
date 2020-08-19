## What are  Ansible Variables & purpose

*  **Ansible offers the possibility to using variables as needed.**

*  **Ansible uses variables to enable more flexibility in any ansible plays.** 

*  **Variable in playbooks are very similar to using variables in any programming language.**

*  **It helps you to use and assign a value to a variable and use that anywhere in the playbook**

*  **One can put conditions around the value of the variables and accordingly use them in the playbook.**

*  **Ansible variables help to determine how the tasks execute on different systems based on the values assigned to these variables.**

*  **Ansible variables can be used to loop through a set of given values, access various information like the hostname of a system and replace certain strings in templates by system specific values etc. ...**

*  **Variables in ansible are available for any ansible play through built in, discovered, inventory, playbooks, variable files & command line.**

```
NOTE:  variable names have some restrictions in Ansible

Variable names should be letters, numbers, and underscores. Variables should always start with a letter.
```


## How & Where to define these variables in Ansible

*  Discovered Variables ( Facts ) 
*  Variables Defined in Inventory
    *   Group variables 
    *   Host variables
*   Variables Defined in Playbooks
*   Built-in Variables
*   Setting Variables on the Command Line
*   Using Variables: Jinja2 ( templates )
*   Variable Precedence: Where Should I Put A Variable?


## Discovered Variables ( Facts )

> Discovered variables (Facts) are not set/defined by user. 

> Variables that are generated from target systems when an ansible play runs are known as discovered variables or Facts

> Facts are information derived from speaking with your remote systems

> We can find a complete set under the ansible_facts variable, most facts are also ‘injected’ as top level variables preserving the ansible_ prefix

> For example, It can be IP address of the remote host, or what the operating system is or which architecture the remote host is etc. … 

> To see what information is available, we can try the following in a play:
```
                  - debug: var=ansible_facts 
```
To see the ‘raw’ information as gathered:
```
                  ansible hostname -m setup
```

```
Example1:                                                   Example2:                                                                 
                                                                                 
- name: print out operating system                          - name: check arch & do some action                                            
  hosts: localhost                                            hosts: localhost                                        
  connection: local                                           connection: local                                             
  gather_facts: True                                          gather_fact: true                                           
  tasks:                                                      tasks:                                
   - debug: var=ansible_distribution                            - command: cat /etc/os-release
   - debug: var=ansible_architecture                              register: out                             
   - debug: var=ansible_default_ipv4.address                    - debug: var=out.stdout == 'Ubuntu'                      
                                                                  when: ansible_distribution == 'SLES'
                                                                - command: echo "hello"
                                                                  register: out
                                                                - debug: var=out.stdout
                                                                  when: ansible_distribution == 'Redhat'
```