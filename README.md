
# Vulnerability Management with OpenVAS

![OpenVAS Logo](https://github.com/redouard2/OpenVAS-VulnLab/assets/73624384/1e792bb3-ac8c-49cb-bf4a-b23a00951e92)



## Summary and Purpose

In this project,I created a secure Azure network and populated it with two virtual machines set to run OpenVAS Vulnerability Management Scanner and Windows 10. The Windows 10 virtual machine was made vulnerable by disabling security controls and installing outdated software. 

To highlight the significance of properly configuring vulnerability scans, two types of scans were conducted. First, an unauthenticated scan was performed on the Windows 10 machine. Following the completion of the unauthenticated scan, a credentialed scan was configured and initiated. Based on the results of the credentialed scan, remediations were implemented to mitigate major vulnerabilities. 

A final credentialed scan was conducted to verify the effectiveness of the implemented remediations.


## Technologies, Azure Components, and Regulations Used

- Azure Virtual Network
- OpenVAS Vulnerability Management Scanner
- Windows 10 Pro virtual machine
- Outdated software known to have vulnerabilities
  - Mozilla Firefox, v97.0b5
  - VideoLAN VLC Media Player, v1.1.7
  - Adobe Reader, v10.0.0


## Deploy Resources and Configure Virtual Machines for an Unauthenticated Scan

The first resource created in this project was OpenVAS Vulnerability Management Scanner. Specifically, OpenVas by HOSSTED was utilized with a default developer configuration. While OpenVAS installed, a Windows 10 Pro virtual machine was created. 

To prepare the Windows 10 machine, the firewall was disabled and outdated versions of Firefox, VLC Media Player, and Adobe Reader were installed. The goal was for the Windows 10 machine to be blatantly vulnerable. Once the firewall was disabled and the vulnerable software was installed, the machine was restarted.


![4 Disable firewall](https://github.com/TylerDeaver/OpenVAS/assets/149614301/d0f4c453-b01f-4bf7-9c45-9844cf4266cc)


<br>To configure OpenVAS for an unauthenticated scan, the following steps were completed:

1. A new host was created by using the Windows 10 virtual machine’s private IP Address.


![1 BEFORE_Uncredentialed host](https://github.com/TylerDeaver/OpenVAS/assets/149614301/80742a9f-0341-4296-bb29-df15529b75a8)


2. A new target was created using the host from the previous step. All other configurations were left as default, and no credentials were provided to OpenVAS.


![2 BEFORE_Uncredentialed target](https://github.com/TylerDeaver/OpenVAS/assets/149614301/694460cc-7de3-40f8-b01f-94e3a28f402f)


3. A new task was created with the target from the previous step. Again, all other configurations were left as default for this sca.


![3 BEFORE_Uncredentialed task](https://github.com/TylerDeaver/OpenVAS/assets/149614301/860b35b1-a330-4eca-8c33-8c662a85d56c)



##  Results of the Unauthenticated Scan

Due to the scan being unauthenticated, the vulnerabilities found do not accurately reflect the vulnerabilities known to be on the machine. The outdated software on the virtual machine is not reflected in this scan due to the limited capabilities inherent in unauthenticated scans.


![5 BEFORE_Uncredentialed overview](https://github.com/TylerDeaver/OpenVAS/assets/149614301/b1c77fa5-e21c-4ed2-8711-f7622f97d1c6)
![6 BEFORE_Uncredentialed reports](https://github.com/TylerDeaver/OpenVAS/assets/149614301/3d5388a7-f94c-4586-8969-161bb15980b7)


## Configure Windows 10 for a Credentialed Scan

Several changes needed to be made to configure the Windows 10 machine for a credentialed scan. The first step was to verify the Domain, Private, and Public profiles for Windows Firewall, which were still disabled from the initial configuration.

The following steps were then completed:

1. Disable User Account Control.


![7 disable user account control](https://github.com/TylerDeaver/OpenVAS/assets/149614301/87eb7d5f-c7e6-429d-b0e5-cb218243a04c)


2. Enable Remote Registry.


![remote registry](https://github.com/TylerDeaver/OpenVAS/assets/149614301/5197899e-c971-44e7-9cca-935b910297ec)


3. Navigate to the Windows Registry and create a new DWORD named “LocalAccountTokenFilterPolicy” and set the value to “1”.


![8 regedit](https://github.com/TylerDeaver/OpenVAS/assets/149614301/bcc9a5df-7068-45ad-bebd-03ed4cb5dbe7)


4. Restart the virtual machine.


## Configure OpenVAS for a Credentialed Scan

While the Windows 10 machine restarted, the following steps were completed to configure OpenVAS for a credentialed scan:

1. Create a new credential by providing the Windows 10 virtual machine’s username and password to OpenVAS.


![9 configure credential](https://github.com/TylerDeaver/OpenVAS/assets/149614301/4e9f8471-cadd-4de3-afba-3cf12b716dc1)


2. Clone the existing target by clicking the sheep icon found under “Actions.” Edit the cloned target and enable SMB by selecting the credentials created in the previous step.


![10 CLONE highlight](https://github.com/TylerDeaver/OpenVAS/assets/149614301/357205d0-d5f2-40cc-811c-a8977671bfc4)
![11 clone target](https://github.com/TylerDeaver/OpenVAS/assets/149614301/925e3d45-49fe-49a5-ab06-92c3a1152e49)



3. Clone the existing task and edit the clone to use the credentialed target created in the previous step.


![12 Credentialed scan clone](https://github.com/TylerDeaver/OpenVAS/assets/149614301/3c1c0fbe-e7b8-41bd-9da9-dd8c21da38ff)


## Results of the Credentialed Scan

The difference in vulnerabilities discovered during the unauthenticated and credentialed scans is night and day. The severity rating increased from 5.0 (medium) to 10.0 (high), and the credentialed scan returned 107 additional vulnerabilities. 

The credentialed scan allowed OpenVAS to evaluate the system fully, which included identifying vulnerabilities in the outdated software. To learn more about the vulnerabilities, OpenVAS provides a tab for Common Vulnerabilities and Exposure (CVE). By including CVEs, OpenVAS allows for an easily digestible breakdown of each vulnerability. The breakdown provided includes a description, the score, vector, references, and remediations.


![13 Credentialed overview](https://github.com/TylerDeaver/OpenVAS/assets/149614301/6461d348-804a-4018-a258-84dce552333d)
![14 Credentialed reports](https://github.com/TylerDeaver/OpenVAS/assets/149614301/570d86ec-41b5-40a0-a290-1188b121a774)
![15 Credentialed CVE](https://github.com/TylerDeaver/OpenVAS/assets/149614301/b1766134-794d-4df1-9f8d-2dbdc6195fb0)
![16 CVE Info](https://github.com/TylerDeaver/OpenVAS/assets/149614301/c871409a-76ad-4efb-8f3b-a9b680467fb4)



## Remediation, Verification Scan, and Conclusion

To remediate most of the vulnerabilities found during the credentialed scan, the outdated software was uninstalled from the Windows 10 machine. Another credentialed scan was performed to verify the implemented remediations resolved the expected vulnerabilities. 

Based on the scan, the remediations were successful, and a downward trend in vulnerabilities was shown. By removing the outdated software, the number of vulnerabilities found by OpenVAS was reduced by 91.


![17 after overview](https://github.com/TylerDeaver/OpenVAS/assets/149614301/a5e53cf9-e572-4b00-8785-68c4a4732440)
![18 after reports](https://github.com/TylerDeaver/OpenVAS/assets/149614301/67f382eb-6787-46c3-80d7-4da865c9d67d)
![19 after cve](https://github.com/TylerDeaver/OpenVAS/assets/149614301/b649e372-3f31-4431-ab58-42f65336375d)
![20 cve info](https://github.com/TylerDeaver/OpenVAS/assets/149614301/934e914e-001e-4607-9ea6-f5d7e4589255)


This project successfully demonstrated the configuration of OpenVAS and the subsequent remediation of vulnerabilities. The project also demonstrated the importance of conducting credentialed scans when possible, as an unauthenticated scan does not accurately reflect the security of a system. Even though additional high-severity vulnerabilities remained on the verification scan, remediating those vulnerabilities was not within this project's scope.


## Reflection
This project was designed to give me hands-on experience with vulnerability management, including configurations and remediations. This was the second Vulnerability Management suite I used (the first being Tenable Nessus), and I have enjoyed doing this project to increase my skills further. In the future, I will be doing a Qualys Vulnerability Management Scanner Lab, redo the Nessus Vulnerability Management Lab, and do my own mini Active Directory Lab.

