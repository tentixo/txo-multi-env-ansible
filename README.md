# Ansible structure for multi-environment projects
This is Tentixo NG AB's standard structure for multi-environment (dev, devsrv, lab, test, stage, prod) Ansible. 

## Directory structure
* `environments/`
  * Top directory for the environment sub-folders
  * YAML files for project global variables `global_env_vars.yml` and credentials `global_env_vars_vault.yml` 
  * Sub-folder have files for environment variables `env_vars.yml` and credentials `env_vars_vault.yml`
  * Use a `tmp/` folder in each environment sub-folder to save files locally blocked by `.gitignore`
* `hosts/`
  * Host INI files for each environment

## Files setup
* Each hosts INI file has a variable `env=` containing the environment name, and herby find the sub-folder name in `environments/`
* All Ansible Playbooks have to have references to the global and environment var files (see example Playbooks)
  ```yaml
    vars:
      env_path: "{{ global_env_path }}/{{ env }}"
    vars_files:
      - "{{ env_path }}/env_vars.yml"
      - "{{ env_path }}/env_vars_vault.yml"
      - "{{ global_env_path }}/global_env_vars.yml"
      - "{{ global_env_path }}/global_env_vars_vault.yml" 
  ```
* Credentials (secrets and passwords) files have to have a name ending in `_vault.yml` or `-secrets.json` so `.gitignore` block it from check-in
* Check in an empty version of your credentials files containing the needed keys, but without the values, see example in this project

## To run a playbook
Point out the environment host file: `ansible-playbook lx-example.yml -i hosts/test.ini`  
As precautionary measure, if you forget to point to a host file, the `hosts/dev.ini` will be run (set in `ansible.cfg`)

/Morre @photomorre 
