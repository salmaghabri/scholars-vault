# Usecases
- for repetitive tasks that sys admins do :
	- deploy distributed app on 10 servers
	- update docker versions on thoese servers
# Benefits
1. execute tasks from your own machines : ==ansible is agentless==: u do not have to install it on target machines -> no deployment effort and no upgrade efforts
2. config/ installation /deployment steps all in one YAML file (yaml simple language too)
3. reuse same file multiple time
4. no human errors
# Components
## Modules
- small programs that do the actual work
- Will be pushed into target hosts via ssh to do the actual work then will be removed.
- very granular and specific : module to install nginx server, to start docker cintainer, create cloud instance ..
## Playbook
- a group of multiple small modules (tasks) in a certain sequences to represent a configuration ( and where should they be executed): orchestration
- **which task** should be executed on **which host** define a Play
- Playbook = multiple plays
- To execute playbooks on target hosts use the “ansible-playbook” command.
## Inventory
- hosts files: all the machines involved in tasks executions
### Roles
provide a consistent approach to organizing playbooks, tasks, and other Ansible artifacts. Offers Reusability, Scalability and Modularity.
# Ansible vault 
allows you to encrypt sensitive data, such as passwords, API keys, within your playbooks. Bu using the command ansible-vault encrypt/ edit/ decrypt [ file_name]
# Ansible Galaxy 
serves as a centralized repository for Ansible automation containing content created by the community and offers several features to streamline the use of Ansible roles and collections