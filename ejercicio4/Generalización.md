
# **Installation and Configuration with Vagrant**

This guide provides detailed steps for setting up and configuring **DECIDE** using **Vagrant** in a development environment. It also includes solutions for specific configuration exercises.

---

## **Table of Contents**
1. [Prerequisites](#prerequisites)
2. [Set Environment Files](#set-environment-files)
3. [Working with Vagrant](#working-with-vagrant)
   - [Run the VM](#run-the-vm)
   - [Accessing the VM](#accessing-the-vm)
   - [Provision the VM](#provision-the-vm)
     - [If the VM is off](#if-the-vm-is-off)
     - [If the VM needs a restart](#if-the-vm-needs-a-restart)
   - [See VM Status](#see-vm-status)
   - [Halt the VM](#halt-the-vm)
   - [Destroy the VM](#destroy-the-vm)
4. [Additional Configuration for DECIDE](#additional-configuration-for-decide)
5. [Exercise Solutions](#exercise-solutions)

---

## **1. Prerequisites**

Ensure that the following tools are installed on your machine:
- **Vagrant**
- **Ansible**
- **VirtualBox**

For production environments, refer to the official [Deployment Guide](#).

---

## **2. Set Environment Files**

1. Copy the `.env.vagrant.example` file to `.env` to configure environment variables:

   ```bash
   cp .env.vagrant.example .env
   ```

2. Be cautious with file and folder permissions. Before starting the machine, delete the following if they exist in the project root:

   ```bash
   rm -r uploads
   rm -r rosemary.egg-info
   rm app.log*
   ```

---

## **3. Working with Vagrant**

### **vagrant folder**
Navigate to the `vagrant` folder in the project root to execute all Vagrant commands:

```bash
cd vagrant
```

---

### **Run the VM**
To start the virtual machine in development mode:

```bash
vagrant up
```

After the VM starts, access **DECIDE** at:  
[http://localhost:5000](http://localhost:5000)

---

### **Accessing the VM**
To access the virtual machine:

```bash
vagrant ssh
```

To exit the VM:

```bash
exit
```

---

### **Provision the VM**

#### **If the VM is off:**
Rerun the provisioning scripts:

```bash
vagrant up --provision
```

#### **If the VM needs a restart:**
Restart the VM and re-provision:

```bash
vagrant reload --provision
```

---

### **See VM Status**
Check the current status of the VM:

```bash
vagrant status
```

---

### **Halt the VM**
Stop the VM:

```bash
vagrant halt
```

---

### **Destroy the VM**
Remove the VM and all its data:

```bash
vagrant destroy
```

Delete the `.vagrant` folder to avoid conflicts with previous configurations:

```bash
rm -r .vagrant
```

---

## **4. Additional Configuration for DECIDE**

### **1. Add a New User**
Modify the Ansible configuration to create a new user `egc` alongside the `decide` user. Update the playbook (e.g., `users.yml`):

```yaml
- name: Create egc user
  ansible.builtin.user:
    name: egc
    state: present
    groups: sudo
```

Commit and push the changes:

```bash
git add .
git commit -m "Add user egc to Ansible configuration"
git push
```

---

### **2. Expose Port 8081**
Modify the `Vagrantfile` to forward port **8081**:

```ruby
config.vm.network "forwarded_port", guest: 8081, host: 8081
```

Commit and push the changes:

```bash
git add Vagrantfile
git commit -m "Expose DECIDE on port 8081"
git push
```

---

### **3. Create a New Branch**
Create and switch to a branch named `vagrant`:

```bash
git checkout -b vagrant
```

---

### **4. Set Admin User**
Update the playbook (e.g., `users.yml`) to set the admin user as `adminexamen`:

```yaml
- name: Set admin user for DECIDE
  ansible.builtin.user:
    name: adminexamen
    state: present
    groups: sudo
```

Commit and push the changes:

```bash
git add .
git commit -m "Set admin user to adminexamen"
git push
```

---

### **5. Configure VM Resources**
Update the `Vagrantfile` to allocate **4 CPUs** and **4 GB of RAM**:

```ruby
config.vm.provider "virtualbox" do |vb|
  vb.memory = "4096"
  vb.cpus = 4
end
```

Commit and push the changes:

```bash
git add Vagrantfile
git commit -m "Set VM resources to 4 CPUs and 4GB RAM"
git push
```

---

## **5. Exercise Solutions**

### 1. **Add a New User (`egc`)**
   - Modify the Ansible configuration to create a new user `egc` alongside the existing `decide` user.
   - Example of changes in `users.yml`:
     ```yaml
     - name: Create egc user
       ansible.builtin.user:
         name: egc
         state: present
         groups: sudo
     ```
   - Commit and push the changes:
     ```bash
     git commit -m "Add user egc to Ansible configuration"
     git push
     ```

### 2. **Expose Port 8081 for DECIDE**
   - Update the `Vagrantfile` to forward port **8081** from the VM to the host system.
   - Example of changes in `Vagrantfile`:
     ```ruby
     config.vm.network "forwarded_port", guest: 8081, host: 8081
     ```
   - Commit and push the changes:
     ```bash
     git commit -m "Expose DECIDE on port 8081"
     git push
     ```

### 3. **Create and Switch to Branch `vagrant`**
   - Create a new branch named `vagrant` and switch to it:
     ```bash
     git checkout -b vagrant
     ```

### 4. **Set Admin User (`adminexamen`)**
   - Update the Ansible playbook to set the admin user as `adminexamen` in the Vagrant environment.
   - Example of changes in `users.yml`:
     ```yaml
     - name: Set DECIDE admin user
       ansible.builtin.user:
         name: adminexamen
         state: present
         groups: sudo
     ```
   - Commit and push the changes:
     ```bash
     git commit -m "Set admin user to adminexamen"
     git push
     ```

### 5. **Configure VM Resources**
   - Modify the `Vagrantfile` to allocate **4 CPUs** and **4 GB of RAM** for the VM.
   - Example of changes in `Vagrantfile`:
     ```ruby
     config.vm.provider "virtualbox" do |vb|
       vb.memory = "4096"
       vb.cpus = 4
     end
     ```
   - Commit and push the changes:
     ```bash
     git commit -m "Set VM resources to 4 CPUs and 4GB RAM"
     git push
     ```


---

Following this guide, you will have a fully configured **DECIDE** environment using **Vagrant** for development.