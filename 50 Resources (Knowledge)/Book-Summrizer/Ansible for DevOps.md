# Ansible for DevOps

> **Author:** Jeff Geerling

---

## ภาพรวมของหนังสือ

หนังสือเล่มนี้สอนการใช้ Ansible (เครื่องมือ Configuration Management) สำหรับ DevOps ตั้งแต่ระดับเริ่มต้นจนถึงขั้นสูง เน้นการจัดการ infrastructure ด้วย code แทน manual process ทั้งหมด

---

## Preface — คำนำ

Jeff Geerling เติบโตมาในวงการ IT จากพ่อที่เป็น chief engineer ของสถานีวิทยุ เขาเริ่มใช้ Linux ก่อนเข้ามัธยม และผ่านประสบการณ์ตั้งแต่ config server ด้วยมือ จนถึงใช้ configuration management tools

หนังสือนี้เหมาะสำหรับ developer และ sysadmin ที่ comfortable กับ Linux server ผ่าน SSH และจัดการ server 1-100 เครื่อง มีประสบการณ์กับ Puppet หรือ Chef บ้าง แต่ไม่จำเป็น

---

## Introduction — ทำไมต้อง Ansible

### ปัญหาของ Manual Server Management
- ยุคแรก sysadmin จัดการ server ทีละเครื่องผ่าน SSH (manual process)
- เมื่อ infrastructure ขยายขึ้น ไม่สามารถ scale manual process ได้
- Shell scripts ซับซ้อนและ handle edge cases ได้ไม่ดี

### ยุค Configuration Management
- เครื่องมืออย่าง CFEngine, Puppet, Chef ได้รับความนิยมช่วง mid-to-late 2000s
- แต่ต้องเรียนรู้ภาษา/ framework ใหม่ (Ruby เป็นต้น)

### จุดเด่นของ Ansible
- **Clear**: ใช้ YAML syntax ที่อ่านง่าย เข้าใจได้ทุกคน
- **Fast**: เรียนเร็ว ติดตั้งง่าย ไม่ต้องติดตั้ง agent/daemon บน server
- **Complete**: ทำได้ 3 อย่างในเครื่องมือเดียว — Configuration Management, Provisioning, Deployment
- **Efficient**: ไม่มี extra software บน server _modules ทำงานผ่าน JSON ขยายได้ง่าย
- **Secure**: ใช้ SSH ไม่ต้องเปิด port เพิ่ม ไม่มี daemon เสี่ยง

### หลักการสำคัญ
- **Idempotence (幂等性)**: รันกี่ครั้งก็ได้ผลลัพธ์เดิม ต่างจาก shell scripts ที่รันซ้ำจะมีผลข้างเคียง
- **Agentless**: ไม่ต้องติดตั้งอะไรบน server เป้าหมาย
- **Push-based**: Ansible controller push คำสั่งไปยัง server ผ่าน SSH

---

## Chapter 1 — เริ่มต้นกับ Ansible

### Snowflake Servers คืออะไร
- Server ที่ config ด้วยมือ ไม่มี documentation สร้างใหม่ยากมาก
- ถ้าต้องการ server ใหม่เหมือนเดิม ต้องนั่งไล่ดูทุก package, config, setting

### Configuration Management
- แก้ปัญหา snowflake server ด้วย tool ที่ทำให้ server ทุกตัวเหมือนกัน
- Ansible สร้างโดย developer/sysadmin ที่เข้าใจ command line

### การติดตั้ง Ansible
- **Mac**: ใช้ Homebrew (`brew install ansible`) หรือ pip (`pip install ansible`)
- **Linux (Debian/Ubuntu)**: `apt-add-repository ppa:ansible/ansible && apt-get install ansible`
- **Linux (RHEL/CentOS)**: ต้องติดตั้ง EPEL repo ก่อน แล้ว `yum install ansible`
- **Windows**: ใช้ Linux VM ผ่าน VirtualBox หรือ Cygwin (unsupported)

### Inventory File
- ไฟล์ที่บอก Ansible ว่ามี server ไหนบ้าง อยู่ที่ `/etc/ansible/hosts`
- รูปแบบ: `[group_name]` แล้ว列出 hostname หรือ IP

### Ad-Hoc Command แรก
- `ansible example -m ping -u [username]` — ทดสอบการเชื่อมต่อ
- `ansible example -a "free -m"` — ดู memory usage บน server ทั้งหมด
- ถ้ามี error ใช้ `-vvvv` เพื่อดู verbose output

---

## Chapter 2 — Local Infrastructure: Ansible + Vagrant

### ทำไมต้อง test บน local VM
- **Safety**: ไม่ต้องกลัวพัง production
- **Speed**: rebuild ได้เร็ว ไม่ต้องรอ provision ใหม่
- **TDD for Infrastructure**: infrastructure ก็ควรถูก test เหมือน code

### Vagrant + VirtualBox
- **Vagrant**: สร้างและจัดการ VM ผ่าน command line
- **VirtualBox**: virtualization engine ที่ใช้รัน VM

### วิธีใช้งาน
1. ติดตั้ง Vagrant + VirtualBox
2. สร้าง folder แล้วรัน `vagrant box add geerlingguy/centos7`
3. `vagrant init geerlingguy/centos7` แล้ว `vagrant up`
4. เชื่อมต่อ VM ด้วย `vagrant ssh`

### Ansible as Vagrant Provisioner
- แก้ Vagrantfile เพิ่ม `config.vm.provision "ansible"` เพื่อให้ Vagrant เรียก Ansible playbook อัตโนมัติ

### Playbook แรก
- ไฟล์ YAML (`playbook.yml`) ที่บอก Ansible ว่าต้อง config อะไรบ้าง
- ตัวอย่าง: ติดตั้ง NTP แล้ว start service — ใช้แค่ 5 บรรทัด
- รันด้วย `vagrant provision`

---

## Chapter 3 — Ad-Hoc Commands

### Ad-Hoc Commands คืออะไร
- คำสั่ง Ansible ที่รันทีเดียวบน server หลายเครื่องพร้อมกัน
- ไม่ต้องเขียน playbook ก็ใช้ได้เลย

### Parallel Execution
- Ansible รันคำสั่งแบบ parallel (พร้อมกัน) โดย default
- ใช้ `-f 1` เพื่อรันทีละเครื่อง (serial mode)
- ใช้ `-f 10` หรือ `-f 25` เพื่อเพิ่ม parallelism

### Multi-Server Infrastructure
- สร้าง 3 VM ด้วย Vagrant: 2 app server + 1 database server
- Inventory file กำหนด groups (`[app]`, `[db]`, `[multi:children]`)

### ตัวอย่าง Ad-Hoc Commands
- **ดูข้อมูล**: `df -h` (disk), `free -m` (memory), `date` (time sync)
- **ติดตั้ง packages**: `ansible app -s -m yum -a "name=django state=present"`
- **จัดการ users**: `ansible app -s -m user -a "name=johndoe group=admin createhome=yes"`
- **จัดการไฟล์**: `copy`, `fetch`, `file` modules
- **Cron jobs**: `ansible multi -s -m cron -a "name='daily' hour=4 job='/path/script.sh'"`
- **Git deploy**: `ansible app -s -m git -a "repo=... dest=/opt/myapp update=yes version=1.2.4"`

### SSH Connection History
- **Paramiko**: SSH2 library สำหรับ Python — default บน OS เก่า
- **OpenSSH**: default ตั้งแต่ Ansible 1.3 — เร็วกว่า ใช้ ControlPersist
- **Accelerated Mode**: เชื่อมต่อครั้งแรกผ่าน SSH แล้วใช้ AES key ผ่าน port 5099 — เร็วขึ้น 2-4x
- **Pipelining**: ส่งคำสั่งผ่าน SSH connection โดยตรงแทน copy file ไปรัน — ต้องตั้ง `pipelining=True` ใน ansible.cfg

---

## Chapter 4 — Ansible Playbooks

### Playbook คืออะไร
- ไฟล์ YAML ที่รวม tasks (plays) ที่ต้องรันบน server
- คล้าย playbook ในอเมริกันฟุตบอล — มี script สำหรับ execute

### Shell Script vs Playbook
- Shell script: ต้องเช็ค if/else เอง, ไม่มี idempotence
- Playbook: ใช้ modules ที่มี built-in idempotence, อ่านง่ายกว่า

### ตัวอย่าง Playbook จริง
1. **CentOS Node.js App Server**: ติดตั้ง Node.js + deploy app บน port 80 ด้วย YAML ไม่ถึง 50 บรรทัด
   - ใช้ `yum`, `npm`, `copy`, `service` modules
   - ใช้ `register` + `when` เพื่อ check state ก่อน start app
2. **Ubuntu LAMP Server + Drupal**: ตั้งค่า Apache, MySQL, PHP, Composer, Drush, Drupal
   - ใช้ `vars_files`, `pre_tasks`, `handlers`, `template`, `lineinfile`
   - Handler จะรันเฉพาะเมื่อ task นั้นมีการ change
3. **Ubuntu Apache Tomcat + Solr**: ติดตั้ง Java search server
   - ใช้ `get_url` ดาวน์โหลดไฟล์, `sha256sum` ตรวจสอบ integrity
   - ใช้ Jinja2 template สำหรับ config files

### คำสั่ง ansible-playbook
- `--limit webservers` — รันแค่ specific group
- `--list-hosts` — ดู hosts ที่จะได้รับผลกระทบ
- `--check` — dry run ไม่ทำจริง
- `--extra-vars "key=value"` — ส่ง variables เพิ่ม

---

## Chapter 5 — Playbooks: Beyond the Basics

### Handlers
- Tasks ที่รันเฉพาะเมื่อมี task อื่น notify เข้ามา (เช่น restart service เมื่อ config เปลี่ยน)
- Notify หลาย handlers ได้, handlers chain ได้
- Handlers รันตอน end of play เท่านั้น ใช้ `meta: flush_handlers` ถ้าต้องการรันกลาง play

### Variables
- **Playbook Variables**: กำหนดใน `vars:` section หรือ `vars_files:`
- **Inventory Variables**: กำหนดใน inventory file หรือ `group_vars/`, `host_vars/`
- **Registered Variables**: เก็บ output ของ task ด้วย `register:`
- **Facts**: ข้อมูลที่ Ansible gather อัตโนมัติจาก server (IP, OS, memory ฯลฯ)
- **Local Facts**: ไฟล์ `.fact` ใน `/etc/ansible/facts.d/`
- **Variable Precedence**: command line (-e) > inventory > playbook vars > facts > role defaults

### Conditionals (when)
- `when: is_db_server` — รัน task เฉพาะเมื่อ variable เป็น true
- ใช้ Jinja2 expressions: `1 in [1,2,3]`, `foo != bar`, `and`, `or`, `not`
- ใช้ Python built-ins: `software_version.split('.')[0] == '4'`

### changed_when / failed_when
- ควบคุมการรายงาน change/failure ของ Ansible
- ตัวอย่าง: Composer ถ้าไม่มีอะไรติดตั้งจะรายงาน "Nothing to install"

### Delegation & Local Actions
- `delegate_to: "{{ monitoring_master }}"` — ส่ง task ไปรันบน host อื่น
- `local_action` — รัน task บน localhost
- `wait_for` — รอจน port/file พร้อมใช้งาน

### Tags
- `--tags "tomcat,say"` — รันแค่ tasks ที่มี tag นั้น
- `--skip-tags "notifications"` — ข้าม tasks ที่มี tag นั้น
- ใช้ได้กับ roles, includes, individual tasks, ทั้ง play

---

## Chapter 6 — Roles and Includes

### Includes
- แยก tasks ออกเป็นไฟล์ย่อย (`- include: tasks/apache.yml`)
- Handlers แยกไฟล์ได้ (`- include: handlers/handlers.yml`)
- Playbooks ซ้อนกันได้ (`- include: web.yml`)

### Roles
- Package ที่รวม tasks, handlers, files, templates, variables เข้าด้วยกัน
- โครงสร้าง role:
  ```
  role_name/
    meta/main.yml      — dependencies
    tasks/main.yml      — tasks
    defaults/main.yml   — default variables (override ได้)
    vars/main.yml       — role variables (override ยาก)
    handlers/main.yml   — handlers
    files/              — static files
    templates/          — Jinja2 templates
  ```

### ตัวอย่าง Role
- Node.js role: ติดตั้ง npm + modules ที่กำหนด
- ใช้ `defaults/main.yml` สำหรับ default values
- Playbook override ได้โดยกำหนด variable ใหม่

### Ansible Galaxy
- Repository ของ community roles ที่พร้อมใช้
- `ansible-galaxy install geerlingguy.apache geerlingguy.mysql geerlingguy.php`
- สร้าง LAMP server ด้วย 6 บรรทัด YAML
- Requirements file (`.txt` หรือ `.yml`) สำหรับ manage dependencies

### Cross-Platform Roles
- ใช้ `include_vars: "{{ ansible_os_family }}.yml"` สำหรับ OS-specific vars
- ใช้ `when: ansible_os_family == 'Debian'` สำหรับ OS-specific tasks

---

## Chapter 7 — Inventories

### Static Inventory
- ไฟล์ที่กำหนด hosts, groups, variables
- ตัวอย่างจริง: Server Check.in — มี groups สำหรับ web, db, log, backup, nodejs

### Non-Prod Environments
- แยก inventory files สำหรับ dev, cert, prod
- รันด้วย `ansible-playbook playbook.yml -i inventory-dev`

### host_vars & group_vars
- ไฟล์ YAML ที่ตั้งชื่อตาม hostname หรือ group name
- ตั้งค่า override variables สำหรับ specific hosts/groups

### Dynamic Inventory
- สำหรับ infrastructure ที่เปลี่ยนแปลงบ่อย (cloud, auto-scaling)
- **DigitalOcean**: ใช้ `digital_ocean.py` script สร้าง inventory จาก API
- **AWS**: ใช้ `ec2.py` script + `ec2_*` modules
- **Custom Dynamic Inventory**: เขียน script (Python/PHP/bash) ที่ output JSON

### add_host & group_by
- `add_host` — เพิ่ม host ใหม่เข้า inventory ระหว่าง playbook run
- `group_by` — สร้าง dynamic groups จาก facts

---

## Chapter 8 — Ansible Cookbooks

### Highly-Available Infrastructure
- ตัวอย่างจริง: Varnish (load balancer) + Apache/PHP (app) + Memcached + MySQL master-slave
- ใช้ Galaxy roles: `geerlingguy.firewall`, `geerlingguy.varnish`, `geerlingguy.apache` ฯลฯ
- Deploy บน Vagrant (local), DigitalOcean, หรือ AWS

### ELK Logging with Ansible
- ตั้งค่า Elasticsearch, Logstash, Kibana ด้วย Ansible
- Forward logs จาก servers อื่นเข้า ELK stack

### GlusterFS
- Distributed filesystem สำหรับ shared storage
- ตั้งค่า distributed volumes ด้วย Ansible

### Mac Provisioning
- ใช้ Ansible รัน playbook บน localhost เพื่อ config Mac
- จัดการ Homebrew packages, dotfiles, system preferences

### Docker-based Infrastructure
- ใช้ Ansible สร้างและจัดการ Docker containers
- ตัวอย่าง: Flask app + MySQL ใน containers แยกกัน

---

## Chapter 9 — Deployments with Ansible

### Deployment Strategies
- **Simple Single-Server**: deploy app บน server เดียว (ตัวอย่าง Ruby on Rails)
- **Zero-Downtime Multi-Server**: ใช้ `serial` rolling update เพื่อไม่ให้ downtime
- **Blue-Green Deployments**: สลับระหว่าง blue/green environments

### Rolling Updates
- `serial: 1` — update server ทีละตัว
- Integration tests หลัง update แต่ละ server เพื่อ verify
- Load balancer แยก server ที่อัปเดตแล้วออกจาก pool ชั่วคราว

---

## Chapter 10 — Server Security and Ansible

### SSH & Remote Access History
- Telnet → rlogin/rsh/rcp → SSH (encrypted, secure)

### Security Best Practices
1. ใช้ encrypted communication (SSH, HTTPS)
2. ปิด root login, ใช้ sudo
3. ลบ software ที่ไม่ใช้ เปิดเฉพาะ port ที่จำเป็น
4. ใช้ least privilege principle
5. Update OS และ software สม่ำเสมอ
6. ตั้ง firewall (uff สำหรับ Debian, firewalld สำหรับ RedHat)
7. Log files ต้อง rotate
8. Monitor logins และบล็อค IP ที่น่าสงสัย
9. ใช้ SELinux หรือ AppArmor

---

## Chapter 11 — Ansible Tower and CI/CD

### Ansible Tower
- Web UI สำหรับจัดการ Ansible playbooks
- มี role-based access control, job scheduling, API
- ทางเลือก: AWX (open source version ของ Tower)

### Jenkins CI
- ใช้ Jenkins run Ansible playbooks อัตโนมัติ
- Create Jenkins job → trigger playbook → deploy

### Testing
- **Unit Tests**: debug module, fail/assert modules
- **Syntax Check**: `ansible-playbook --syntax-check`
- **Dry Run**: `ansible-playbook --check`
- **Travis CI**: ทดสอบ roles อัตโนมัติบน GitHub
- **Serverspec**: functional testing สำหรับ infrastructure

---

## Appendix A — Using Ansible on Windows

- ติดตั้ง Linux VM บน VirtualBox (Ubuntu)
- ติดตั้ง Ansible บน VM
- ใช้ VM เป็น Ansible controller เชื่อมต่อ server อื่น

---

## Appendix B — Best Practices

### Playbook Organization
- เขียน comments และใช้ `name` เสมอ
- รวม variables และ tasks ที่เกี่ยวข้อง
- ใช้ Roles เพื่อ bundle configuration
- ใช้ `defaults/main.yml` สำหรับ default values

### YAML Conventions
- 3 รูปแบบ: shorthand (`key=value`), structured map, folded scalars (`>`)
- ใช้ `|` สำหรับ multiline variables

### แนวทางปฏิบัติ
- ใช้ Ansible Tower สำหรับ teams
- ตั้ง `--forks` สำหรับ servers มากกว่า 5 เครื่อง
- ใช้ ansible.cfg สำหรับ configuration

---

## สรุปภาพรวม

หนังสือเล่มนี้ครอบคลุม Ansible ตั้งแต่ต้นจนจบ — ตั้งแต่ติดตั้ง, ad-hoc commands, playbooks, roles, inventories, deployments, security, CI/CD จนถึง best practices มีตัวอย่างจริงจาก production infrastructure ของผู้เขียนเอง เหมาะสำหรับ developer และ sysadmin ที่ต้องการจัดการ infrastructure ด้วย code อย่างมีประสิทธิภาพ
---
