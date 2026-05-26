# POC for Jenkins Setup

---

| Author      | Created    | Version | Last Updated By | Last Edited On | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 2026-05-26 | 1.0     | Saransh Rai     | 2026-05-26     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Jenkins Setup POC](#4-jenkins-setup-poc)
   * [4.1 Update System Packages](#41-update-system-packages)
   * [4.2 Install Java](#42-install-java)
   * [4.3 Add Jenkins Repository Key](#43-add-jenkins-repository-key)
   * [4.4 Add Jenkins Repository](#44-add-jenkins-repository)
   * [4.5 Install Jenkins](#45-install-jenkins)
   * [4.6 Start and Enable Jenkins](#46-start-and-enable-jenkins)
   * [4.7 Check Jenkins Status](#47-check-jenkins-status)
   * [4.8 Allow Jenkins Port](#48-allow-jenkins-port)
   * [4.9 Get Initial Admin Password](#49-get-initial-admin-password)
   * [4.10 Access Jenkins in Browser](#410-access-jenkins-in-browser)
   * [4.11 Complete Jenkins Setup](#411-complete-jenkins-setup)
5. [Jenkins Workflow Diagram](#5-jenkins-workflow-diagram)
6. [Validation](#6-validation)
7. [Best Practices](#7-best-practices)
8. [Conclusion](#8-conclusion)
9. [Contact Information](#9-contact-information)
10. [Jenkins Steup Documentation](#10-jenkins-steup-documentation)
11. [References](#11-references)

---

# 1. Introduction

This document provides a Proof of Concept (POC) for setting up Jenkins on an Ubuntu server using the normal installation method. The setup includes Java installation, Jenkins repository configuration, package installation, service validation, and accessing the Jenkins web interface.

---

# 2. Objective

The objective of this POC is to install and configure Jenkins successfully on Ubuntu and validate that Jenkins is running properly on port `8080`.

---

# 3. Prerequisites

| Requirement           | Description                       |
| --------------------- | --------------------------------- |
| Operating System      | Ubuntu Server                     |
| User Access           | Sudo or root access               |
| Internet Connectivity | Required for downloading packages |
| Open Port             | Port `8080` should be accessible  |
| Java                  | Required dependency for Jenkins   |

---

# 4. Jenkins Setup POC

## <a name="41-update-system-packages"></a>    4.1 Update System Packages

```bash
sudo apt update
```

<details>
<summary>Click to View Update System Packages</summary>

<img width="1918" height="1007" alt="image" src="https://github.com/user-attachments/assets/dd1c4ff3-82c7-456c-a64d-3822492fee10" />


</details>


This command updates the package index and ensures the latest package information is available.

---

## <a name="42-install-java"></a>    4.2 Install Java

```bash
sudo apt install fontconfig openjdk-21-jre -y
```

<details>
<summary>Click to View Install Java</summary>

<img width="1917" height="968" alt="image" src="https://github.com/user-attachments/assets/00a4baa0-fe3d-48c3-9585-3e426f774146" />

</details>


Verify Java installation:

```bash
java -version
```

<details>
<summary>Click to View Verify Java installation</summary>

<img width="957" height="95" alt="image" src="https://github.com/user-attachments/assets/094ccde5-802f-4338-a189-21faebffb8d7" />


</details>

Expected output:

```text
openjdk version "21"
```

---

## <a name="43-add-jenkins-repository-key"></a>    4.3 Add Jenkins Repository Key

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

<details>
<summary>Click to View Added Jenkins Repository Key</summary>

<img width="1880" height="283" alt="image" src="https://github.com/user-attachments/assets/1eef7d4c-78a7-4c12-83ba-935f17b8948a" />


</details>


This command downloads and stores the Jenkins GPG repository key securely.

---

## <a name="44-add-jenkins-repository"></a>    4.4 Add Jenkins Repository

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

This adds the Jenkins repository to the Ubuntu package source list.

<details>
<summary>Click to View Added Jenkins Repository</summary>

<img width="1897" height="70" alt="image" src="https://github.com/user-attachments/assets/99bb0d0e-fd65-431e-9db5-6126595c94fd" />


</details>


---

## <a name="45-install-jenkins"></a>    4.5 Install Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
```

This installs the Jenkins package and its dependencies.

<details>
<summary>Click to View Install Jenkins</summary>

<img width="1873" height="958" alt="image" src="https://github.com/user-attachments/assets/d3d7df82-c8f2-4fcb-ac3b-30307c854d8f" />


</details>

---

## <a name="46-start-and-enable-jenkins"></a>    4.6 Start and Enable Jenkins

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

This starts the Jenkins service and ensures it starts automatically after reboot.

<details>
<summary>Click to View Verify The Start and Enable Jenkins</summary>

<img width="1348" height="135" alt="image" src="https://github.com/user-attachments/assets/3ae0b9d4-5e9d-42f8-9f32-c4d9e12b4b2c" />


</details>

---

## <a name="47-check-jenkins-status"></a>    4.7 Check Jenkins Status

```bash
sudo systemctl status jenkins
```

Expected result:

```text
active (running)
```

This validates that Jenkins is running successfully.

<details>
<summary>Click to View Verify Jenkins Status</summary>

<img width="1907" height="522" alt="image" src="https://github.com/user-attachments/assets/2c00fdf8-bc90-4376-bdf6-db55e9086947" />


</details>

---

## <a name="48-allow-jenkins-port"></a>    4.8 Allow Jenkins Port

```bash
sudo ufw allow 8080
sudo ufw status
```

This allows incoming traffic on Jenkins default port `8080`.

<details>
<summary>Click to View Verify - Allow Jenkins Port</summary>

<img width="702" height="157" alt="image" src="https://github.com/user-attachments/assets/6da6e0d4-e5cf-4288-8644-bad9bd2c8981" />
<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/11dd726d-f3fb-430d-a167-9375b0fb0503" />


</details>

---

## <a name="49-get-initial-admin-password"></a>    4.9 Get Initial Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

This retrieves the Jenkins initial admin password required for first-time login.

<details>
<summary>Click to View Verify Java installation</summary>

<img width="1052" height="88" alt="image" src="https://github.com/user-attachments/assets/0d931eaf-14a4-4349-a8c8-07d460625d04" />


</details>

---

## <a name="410-access-jenkins-in-browser"></a>    4.10 Access Jenkins in Browser

Open Jenkins using:

```text
http://<server-public-ip>:8080
```

The Jenkins unlock page should appear.

<details>
<summary>Click to View Verify Java installation</summary>

<img width="1907" height="972" alt="image" src="https://github.com/user-attachments/assets/dc300f57-222a-4fd6-9e8b-21c7a9ea4cd3" />

<img width="1910" height="957" alt="image" src="https://github.com/user-attachments/assets/ad50338f-80b9-4ed0-a5e1-87d323833f6d" />

<img width="1918" height="975" alt="image" src="https://github.com/user-attachments/assets/c2b504c9-43a6-4cd7-8a71-56cbf0a84d5e" />

<img width="1917" height="957" alt="image" src="https://github.com/user-attachments/assets/ab9b6c10-552b-4a8a-bdfd-5ef3fe75867c" />

<img width="1918" height="972" alt="image" src="https://github.com/user-attachments/assets/17a5c79d-33ac-4060-af62-282c8645bf00" />


</details>

---

## <a name="411-complete-jenkins-setup"></a>    4.11 Complete Jenkins Setup

1. Paste the initial admin password.
2. Select **Install Suggested Plugins**.
3. Create the first admin user.
4. Save and continue.
5. Open Jenkins dashboard.

---

# 5. Jenkins Workflow Diagram

The following workflow explains the complete Jenkins setup process starting from server preparation to accessing the Jenkins dashboard in the browser.

```mermaid
flowchart TB

%% ---------------- SERVER PREPARATION ---------------- %%
subgraph SERVER_PREPARATION["Server Preparation"]
    
    A["🖥️ Ubuntu Server"]

    B["📦 Update System Packages
    sudo apt update"]

    C["☕ Install Java Runtime
    OpenJDK 21"]

    A --> B --> C

end

%% ---------------- JENKINS INSTALLATION ---------------- %%
subgraph JENKINS_INSTALLATION["Jenkins Installation"]

    D["🔐 Add Jenkins GPG Key"]

    E["📁 Add Jenkins Repository"]

    F["⚙️ Install Jenkins Package
    sudo apt install jenkins -y"]

    D --> E --> F

end

%% ---------------- SERVICE CONFIGURATION ---------------- %%
subgraph SERVICE_CONFIGURATION["Service Configuration"]

    G["🚀 Start Jenkins Service
    systemctl start jenkins"]

    H["🔄 Enable Jenkins at Boot
    systemctl enable jenkins"]

    I["✅ Verify Jenkins Status
    systemctl status jenkins"]

    G --> H --> I

end

%% ---------------- NETWORK CONFIGURATION ---------------- %%
subgraph NETWORK_CONFIGURATION["Network Configuration"]

    J["🌐 Allow Firewall Port 8080
    sudo ufw allow 8080"]

end

%% ---------------- JENKINS ACCESS ---------------- %%
subgraph JENKINS_ACCESS["Jenkins Initial Setup"]

    K["🔑 Retrieve Initial Admin Password"]

    L["🌍 Access Jenkins UI
    http://server-ip:8080"]

    M["🧩 Install Suggested Plugins"]

    N["👤 Create Jenkins Admin User"]

    O["🎉 Jenkins Dashboard Ready"]

    K --> L --> M --> N --> O

end

%% ---------------- MAIN FLOW ---------------- %%
C --> D
F --> G
I --> J
J --> K
```

## Workflow Explanation

| Step | Description |
|------|-------------|
| Ubuntu Server | Jenkins setup starts on an Ubuntu server with sudo access. |
| Update System Packages | `sudo apt update` refreshes package metadata from Ubuntu repositories. |
| Install Java Runtime | Jenkins requires Java to run because Jenkins is Java-based software. |
| Add Jenkins GPG Key | The Jenkins repository signing key is added for package verification and security. |
| Add Jenkins Repository | Official Jenkins repository is added to Ubuntu package sources. |
| Install Jenkins Package | Jenkins package and dependencies are installed using APT package manager. |
| Start Jenkins Service | Jenkins service is started using `systemctl start jenkins`. |
| Enable Jenkins Service | Ensures Jenkins automatically starts after server reboot. |
| Verify Jenkins Status | Validates Jenkins is running successfully using systemctl status. |
| Allow Port 8080 | Firewall rule is added so Jenkins UI can be accessed externally. |
| Retrieve Initial Admin Password | Jenkins generates a one-time admin password during first startup. |
| Access Jenkins in Browser | User opens Jenkins web interface using server public IP and port `8080`. |
| Install Suggested Plugins | Jenkins installs default recommended plugins for CI/CD operations. |
| Create Admin User | First administrator account is created for Jenkins access management. |
| Jenkins Dashboard Ready | Jenkins setup is completed successfully and dashboard becomes accessible. |

---

# 6. Validation

| Validation Point  | Command / Check                 | Expected Output                  |
| ----------------- | ------------------------------- | -------------------------------- |
| Java Installation | `java -version`                 | Java version displayed           |
| Jenkins Service   | `sudo systemctl status jenkins` | `active (running)`               |
| Jenkins Port      | `sudo ss -tulnp \| grep 8080`   | Jenkins listening on port `8080` |
| Jenkins UI        | `http://<server-ip>:8080`       | Jenkins setup page accessible    |

---

# 7. Best Practices

| Best Practice                 | Description                            |
| ----------------------------- | -------------------------------------- |
| Use LTS Version               | Prefer Jenkins LTS for stability       |
| Secure Firewall               | Allow only required ports              |
| Backup Jenkins Home           | Protect Jenkins configuration and jobs |
| Use Strong Passwords          | Secure Jenkins admin account           |
| Install Required Plugins Only | Reduce unnecessary dependencies        |

---

# 8. Conclusion

This POC demonstrates a successful normal installation of Jenkins on Ubuntu using the official Jenkins repository. Java was installed as a prerequisite, Jenkins service was configured and validated, and the Jenkins web interface was accessed successfully for initial setup.

---

# 9. Contact Information

| Name        | Contact Type | Details                                                                         |
| ----------- | ------------ | ------------------------------------------------------------------------------- |
| Saransh Rai | Email        | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 10. Jenkins Steup Documentation

| Title                                                                                     | Description                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Jenkins Setup Documentation](https://github.com/Snaatak-Infra-Titans/Documentations) | This documentation contains the complete Jenkins setup POC including installation steps, validation, architecture understanding, service configuration, and Infrastructure as Code concepts. |

---

# 11. References

| Title                                                                                 | Description                                      |
| ------------------------------------------------------------------------------------- | ------------------------------------------------ |
| [Jenkins Official Documentation](https://www.jenkins.io/doc/)                         | Official Jenkins documentation and setup guides  |
| [Jenkins Linux Installation Guide](https://www.jenkins.io/doc/book/installing/linux/) | Step-by-step Jenkins installation on Linux       |
| [OpenJDK Documentation](https://openjdk.org/)                                         | Java runtime documentation                       |
| [Ubuntu Package Management](https://help.ubuntu.com/community/AptGet/Howto)           | Ubuntu package installation and management guide |
