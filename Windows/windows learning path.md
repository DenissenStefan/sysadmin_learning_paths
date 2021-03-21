# Windows Learning Path

1.  Install virtualisation software, for example Hyper-V, ESXI or VMware workstation.
2.  Make a VM with at least 3 NICs, this is gonna be the firewall, you have 2 options:
    1.  (Preffered) Install PFSENSE and use this as your firewall/router
    2.  Install Windows-server (Gui-Mode), configure this with windows routing.
3.  Configure a WAN side a Server side and a desktop side. optional: create a DMZ vlan
4.  Make a VM with a single NIC on the server VLAN
5.  Configure an Active Directory Domain Controller in GUI mode.
6.  Make another server, install this one as a core server. Make this a domain controller as well. (Same domain)
7.  Create an Active Directory Certificate Authority.
8.  Test replication of the active directory through the GUI and CLI.
9.  Make an Organisational Unit (OU) for your workstations, users and groups. All new computers or user accounts should be stored in the right OU.  DO NOT REMOVE THE DOMAIN CONTROLLERS OUT OF THE DC OU!
10. Configure DNS, create a reverse look-up zone.
11. DNS records get old and can ruin name-to-ip resolution. Make sure that DNS records get automaticly scavenged.
12. Block facebook.com through the windows DNS. Make sure that when a DNS lookup is being resolved that computers use a loopback address. Test this through the CLI with cmd.exe.
13. Configure DHCP on the first domain controller.
14. Configure a scope that distributes IP adresses for the desktop VLAN. Make sure that the DHCP scope gives the endpoint the information it needs to join the domain.
15. Install a windows 10 host on the desktop VLAN
16. Your dekstops aren't getting an IP, why is this (hint: routing/broadcast/relay issues).
17. Join the computer to the domain.
18. Now while the DHCP scope is working take the time to configure DHCP clustering, either choose loadbalancing or failover. do test this!
19. Create a non-domain admin account in AD. fill the entire profile when the account is created.
20. Login on the desktop as a regular AD user and as an admin user. try to install some software with the non-admin account first. Try the admin account second. What is the difference?
21. Make a non-admin account. configure this account as a local admin on the desktop. who else is a local admin?
22. Look at the attributes of the account in AD, you'll need to enable advanced features for this.
23. Create an AD group, add the first non-admin account to the group.
24. Install RSAT tools on the desktop which will enable you to remotely manage other computers & servers.
25. Configure remote management on the core server to enable management from the MMC of other computers (there are multiple options)
26. Check which servers has the FSMO role with both the GUI and CLI.
27. Split the FSMO roles through the servers. Try to keep the forest level and domain level in the same spot.
28. Create a file share on the domain controller with gui. configure this so only the administrators and the second non-admin account have access to this fileshare. Create another folder and only grant permission to the created AD group.
29. Use group policy to map both shares as network drives.
30. Login on the workstation as the first domain user. Do you see the mapped network drive in windows explorer? If not, use GPresult to find out why. If you see the mapped drive, try to access the drive. You shouldn't be able to access if the permissions are configured right. Try to log in with the second domain user account.
31. What happens when the account in the group tries to access the second drive. This should work. Login on the workstation with the second non-admin account. This account should not have access because it isn't part of the group. Don't log off. Add the account to the group. Can you access the network drive now? Not? Log off and log in again.
32. Delete the share from the domain controller, we don't want shares on the domain controller if not neccesary.
33. Create another two servers and join these to the domain as member servers.
34. Install DFS and file server roles/features on both servers.
35. Create a file share on both servers with the same folder name. Create files on both servers and make sure they differ from each other. (for example: server 1 has TextDoc01.txt and server 2 has TextDoc02.txt in the shares)
36. Create a DFS namespace and add the shares to the name space.
37. Navigate to the DFS namespace from a domain joined workstation. You should be able to see both files.
38. Make a DFS recplication group. Ensure you have two-way replication. You should be able to see both files on both servers. Make a change in the fileshare on one of the servers and check if this change is being replicated to the other server.
    1.  The file servers can be disabled for now.
39. Create an exchange server. make sure you have fully working internal webmail with TLS.
40. Create a second exchange server. make sure it works in a DAG with redundant database copies.
41. Create a server with System Center Operations Manager. Configure monitoring that sends alerts as emails.
    1.  Bonus point for SNMP monitoring and syslog.
42. Create a server with system center data protection manager. Create a backup job, delete and recreate a VM from backup.
43. Configure SCOM reporting with web interface & dashboards (this is complex!)
44. Create a log rotation/retention policy in AD for the log files on individual servers. This as log rotation to compress logs and archive old legs. (SCOM will take care of long term retention and analysis)
45. Create a WSUS server and deploy this to your workstations with a GPO.
    1.  Bonus points: install WSUS on another server and create an upstream & downstream server (DMZ VLAN!)
46. Configure groups in WSUS, servers and workstations will do.
47. Create a new GPO which will point workstations to the WSUS workstation group and servers to the WSUS server group.
48. Check how you can add updates in an air-gapped network.
49. Create a WDS server and configure this so you can PXE boot to the network. Make the needed changes in routing and DHCP.
50. Upload an image to WDS for PXE deployment. use WAIK and sysprep where needed.
51. Create a desktop VM and install with PXE.
52. Configure WDS and MDT for PXE and media-based imaging. Learn about task sequences.
53. Create a light touch image and configure PXE boot from WDS so it will point to this image.
54. Try configuring a zero touch image.
55. Create a server with system center configuration manager (SCCM) and make this responsible for updates, disable the WSUS GPO and enable the software update point for clients managed through SCCM.
56. Configure SCCM for system imaging task sequences. Learn about WIMs instead of updating, PXE booting, etc. Integrate MDT in SCOM.
57. SCCM application deployment and updating.
58. SCCM configuration items and baselines
59. Do the following on the domain controller or computer with RSAT installed, only using powershell:
60. Create new AD user.
61. Create new AD group
62. Print an overview with just the names of all domain users and computers.
63. Crea an overview of all users with New York as offic. If non-existing, use powershell to configure these attributes.
64. Remove an user account from AD.
65. Add an user to a group.
66. Provision new users with the help of a CSV.
67. Get addicted to automation, all the old checklists and procedures become templates for automation.
68. Download PDQ inventory & deploy trial on a server and automate the break/fix side of helpdesk work.
69. If a problem arises, find a solution with powershell or CMD. Create a package with PDQ deploy that mirrors every manual step of the fix.
70. Create a dynamic collection with PDQ inventory which filters for the known problem.
71. Point to the 'fixit' package, every machine with the same problem should get the fix automaticaly.
72. Configure a network monitor. most of the options are linux based.
73. Research OMD (from bananian) and check MK, install on a raspberry pi.
74. Research and install openstack. (Use this with the help of DevStack https://docs.openstack.org/devstack/latest/ , fllowed by FUEL https://wiki.openstack.org/wiki/Fuel )
75. Learn powershell DSC for provisioning and mitigating change drift of windows servers.
76. Nagios/ solarwinds for help desk and alerting.
77. Forward logs to something like greylog.
78. Learn about https://www.turnkeylinux.org/ and think of new ways to solve business problems.
79. Start anew and install ProxMox
80. Install Windows subsystem for linux on your workmachine
81. Install ansible on your new bash shell
82. Configure your proxmox server with just the webbrowser. Only ise LXC's while creating playbooks to automate everything. Save everything in github.

Source: https://www.reddit.com/r/sysadmin/comments/3z7qd9/practice_to_become_a_windows_sysadmin/cyjynxh/ from /u/gex80