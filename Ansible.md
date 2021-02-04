### Ansible

- 서버의 provisioning, SW deployment 자동화 툴
- Agentless: 비슷한 다른 자동화 도구와 달리 agent 기반 pull 방식이 아니라 타겟 대상에 agentless push 방식으로 동작(관리 서버에만 Ansible을 설치하면 됨)
- 한 번 설치한 것은 중복해서 다시 설치하지 않음



- 구성 요소

  - inventory
    - 대상, 즉 host ip들을 정리한 파일
    - 별명, 그룹 지정, SSH접근 방식 등을 기록
  - playbook
    - inventory에 작성된 서버들을 대상으로 수행할 특정 행위에 대해 정의한 파일(.yaml)

  => 공통적으로 사용되는 변수, 행위 등을 task 단위로 따로 분리, 저장해 다양한 배포 버전을 만들어 낼 수 있음



- 실행

  ```
  ansible-playbook <playbook file> (-i <inventory file> : cfg에 있는 host 목록 활용하지 않는 경우)
  ```



____

- File modules

  - [acl -- Sets and retrieves file ACL information.](https://docs.ansible.com/ansible/2.4/acl_module.html)
  - [archive -- Creates a compressed archive of one or more files or trees.](https://docs.ansible.com/ansible/2.4/archive_module.html)
  - [assemble -- Assembles a configuration file from fragments](https://docs.ansible.com/ansible/2.4/assemble_module.html)
  - [blockinfile -- Insert/update/remove a text block surrounded by marker lines.](https://docs.ansible.com/ansible/2.4/blockinfile_module.html)
  - [copy -- Copies files to remote locations](https://docs.ansible.com/ansible/2.4/copy_module.html)
  - [fetch -- Fetches a file from remote nodes](https://docs.ansible.com/ansible/2.4/fetch_module.html)
  - [file -- Sets attributes of files](https://docs.ansible.com/ansible/2.4/file_module.html)
  - [find -- Return a list of files based on specific criteria](https://docs.ansible.com/ansible/2.4/find_module.html)
  - [ini_file -- Tweak settings in INI files](https://docs.ansible.com/ansible/2.4/ini_file_module.html)
  - [iso_extract -- Extract files from an ISO image](https://docs.ansible.com/ansible/2.4/iso_extract_module.html)
  - [lineinfile -- Ensure a particular line is in a file, or replace an existing line using a back-referenced regular expression.](https://docs.ansible.com/ansible/2.4/lineinfile_module.html)
  - [patch -- Apply patch files using the GNU patch tool](https://docs.ansible.com/ansible/2.4/patch_module.html)
  - [replace -- Replace all instances of a particular string in a file using a back-referenced regular expression.](https://docs.ansible.com/ansible/2.4/replace_module.html)
  - [stat -- Retrieve file or file system status](https://docs.ansible.com/ansible/2.4/stat_module.html)
  - [synchronize -- A wrapper around rsync to make common tasks in your playbooks quick and easy.](https://docs.ansible.com/ansible/2.4/synchronize_module.html)
  - [tempfile -- Creates temporary files and directories.](https://docs.ansible.com/ansible/2.4/tempfile_module.html)
  - [template -- Templates a file out to a remote server](https://docs.ansible.com/ansible/2.4/template_module.html)
  - [unarchive -- Unpacks an archive after (optionally) copying it from the local machine.](https://docs.ansible.com/ansible/2.4/unarchive_module.html)
  - [xattr -- set/retrieve extended attributes](https://docs.ansible.com/ansible/2.4/xattr_module.html)
  - [xml -- Manage bits and pieces of XML files or strings](https://docs.ansible.com/ansible/2.4/xml_module.html)

  

- Command modules

  - If you want to run a command through the shell (say you are using `<`, `>`, `|`, etc), you actually want the [ansible.builtin.shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module) module instead. Parsing shell metacharacters can lead to unexpected commands being executed if quoting is not done correctly so it is more secure to use the `command` module when possible.

  - `creates`, `removes`, and `chdir` can be specified after the command. For instance, if you only want to run a command if a certain file does not exist, use this.
  - 

  ```yaml
  - name: Return motd to registered var
    command: cat /etc/motd
    register: mymotd
  
  # free-form (string) arguments, all arguments on one line
  - name: Run command if /path/to/database does not exist (without 'args')
    command: /usr/bin/make_database.sh db_user db_name creates=/path/to/database
  
  # free-form (string) arguments, some arguments on separate lines with the 'args' keyword
  # 'args' is a task keyword, passed at the same level as the module
  - name: Run command if /path/to/database does not exist (with 'args' keyword)
    command: /usr/bin/make_database.sh db_user db_name
    args:
      creates: /path/to/database
  
  # 'cmd' is module parameter
  - name: Run command if /path/to/database does not exist (with 'cmd' parameter)
    command:
      cmd: /usr/bin/make_database.sh db_user db_name
      creates: /path/to/database
  
  - name: Change the working directory to somedir/ and run the command as db_owner if /path/to/database does not exist
    command: /usr/bin/make_database.sh db_user db_name
    become: yes
    become_user: db_owner
    args:
      chdir: somedir/
      creates: /path/to/database
  
  # argv (list) arguments, each argument on a separate line, 'args' keyword not necessary
  # 'argv' is a parameter, indented one level from the module
  - name: Use 'argv' to send a command as a list - leave 'command' empty
    command:
      argv:
        - /usr/bin/make_database.sh
        - Username with whitespace
        - dbname with whitespace
      creates: /path/to/database
  
  - name: Safely use templated variable to run command. Always use the quote filter to avoid injection issues
    command: cat {{ myfile|quote }}
    register: myoutput
  ```

  

- Shell modules

  ```yaml
  - name: Execute the command in remote shell; stdout goes to the specified file on the remote
    ansible.builtin.shell: somescript.sh >> somelog.txt
  
  - name: Change the working directory to somedir/ before executing the command
    ansible.builtin.shell: somescript.sh >> somelog.txt
    args:
      chdir: somedir/
  
  # You can also use the 'args' form to provide the options.
  - name: This command will change the working directory to somedir/ and will only run when somedir/somelog.txt doesn't exist
    ansible.builtin.shell: somescript.sh >> somelog.txt
    args:
      chdir: somedir/
      creates: somelog.txt
  
  # You can also use the 'cmd' parameter instead of free form format.
  - name: This command will change the working directory to somedir/
    ansible.builtin.shell:
      cmd: ls -l | grep log
      chdir: somedir/
  
  - name: Run a command that uses non-posix shell-isms (in this example /bin/sh doesn't handle redirection and wildcards together but bash does)
    ansible.builtin.shell: cat < /tmp/*txt
    args:
      executable: /bin/bash
  
  - name: Run a command using a templated variable (always use quote filter to avoid injection)
    ansible.builtin.shell: cat {{ myfile|quote }}
  
  # You can use shell to run other executables to perform actions inline
  - name: Run expect to wait for a successful PXE boot via out-of-band CIMC
    ansible.builtin.shell: |
      set timeout 300
      spawn ssh admin@{{ cimc_host }}
  
      expect "password:"
      send "{{ cimc_password }}\n"
  
      expect "\n{{ cimc_name }}"
      send "connect host\n"
  
      expect "pxeboot.n12"
      send "\n"
  
      exit 0
    args:
      executable: /usr/bin/expect
    delegate_to: localhost
  
  # Disabling warnings
  - name: Using curl to connect to a host via SOCKS proxy (unsupported in uri). Ordinarily this would throw a warning
    ansible.builtin.shell: curl --socks5 localhost:9000 http://www.ansible.com
    args:
      warn: no
  ```