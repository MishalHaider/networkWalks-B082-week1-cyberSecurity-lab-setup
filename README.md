# networkWalks-B082-week1-cyberSecurity-lab-setup
First Week Project during the internship at "Network Walks".Building the setup for CyberSecurity Lab.
                                      **The Problem**
**Missing IPv4 Address :**
~After turning the eth0 interface down and back up via terminal commands, NetworkManager dropped the active IP configuration.
Even after manually entering 10.0.0.2 in the GUI settings, NetworkManager failed to bind the IP to the interface, leaving eth0 without an IPv4 address (only an inet6 link-local address was showing).

<img width="328" height="327" alt="prob" src="https://github.com/user-attachments/assets/fc937707-483e-4a23-ba4d-7bc388af12c2" />


**Solution**
Bypassing NetworkManager via Terminal: Instead of relying on the GUI settings to negotiate the static IP, we manually forced the IP address, subnet mask, and gateway directly through the command line.
