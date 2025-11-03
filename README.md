<h1>Create a Linux User and Group on a windows system using PuTTy to connect via SSH  to the Linux system</h1>

<h2>Description</h2>
<ul>
  <li>Demonstration on connecting to a Linux system through a windows system via SSH using the PuTTy application.</li>
  <li>Through the SSH connection, create a new group in Linux and add a new user to that group.</li>
</ul>

<h2>Languages and Utilities Used</h2>

- <b>Kali Linux, Linux Users and Groups, PowerShell</b> 

<h2>Environments Used </h2>

- <b>Windows 11, PuTTy, VirtualBox, Kali Linux</b>

<h2>Program walk-through:</h2>

<h3>Part 1: Install PuTTy with powershell
</h3>

<p>PuTTy is a free open-source terminal emulator widely used for connecting securely to and managing remote computers. It supports several protocols for remote connections, including SSH , Telnet, Rlogin, Serial, and RAW. It also provides command-line tools for secure file transfers like PSCP (for SCP) and PSFTP (for SFTP).</p>

<br />

<p>Open powershell and enter the following command to install putty</p>
<ul>
  <li>winget search putty</li>
  <li>Type yes to agree to all the source agreements terms</li>
</ul>

<img src="https://i.imgur.com/9Y1IuAi.png" height="80%" width="80%" alt="winget search putty"/>

<p>Notice the second line PuTTY.PuTTY Id, version 0.83.0.0, that is the current version available. </p>

<p>Copy the Id and paste it in the following command to install PuTTy</p>
<ul>
  <li>winget install PuTTY.PuTTY</li>
</ul>

<img src="https://i.imgur.com/doMyvOa.png" height="80%" width="80%" alt="winget install putty"/>

<p>PuTTy has been successfully installed.</p>
<ul>
  <li>exit command to close powershell</li>
</ul>

<h3>Part 2: Connect to Kali Linux VM with PuTTy (on the same network)
</h3>

<p>Open a kali linux vm, change the connection to Bridged Adaptor. This makes the VM act as another physical device on the network, allowing it to obtain an IP address from your router and communicate with other devices.</p>

<img src="https://i.imgur.com/zfefvzk.png" height="80%" width="80%" alt="kali vm network settings"/>

<p>Open kali linux vm</p>

<br />

<p>Open the terminal emulator</p>

<br />

<p>Enter the following command to get the kali vm IP</p>
<ul>
  <li>ifconfig</li>
</ul>

<img src="https://i.imgur.com/hRL0xem.png" height="80%" width="80%" alt="kali ifconfig"/>

<p>Note: Under eth0, inet: that is the IP of the kali system</p>

<p>Open powershell on the windows system and ping the Kali VM IP to verify the windows system and the Kali VM can communicate with eachother</p>
<ul>
  <li>ping [enter Kali VM IP here]</li>
</ul>

<img src="https://i.imgur.com/zuLVyh6.png" height="80%" width="80%" alt="ping kali from powershell"/>

<p>Notice the reply, that means both systems are communicating</p>

<p>Verify SSH is enabled on Linux machine enter the following command</p>
<ul>
  <li>sudo systemctl status ssh</li>
</ul>

<img src="https://i.imgur.com/rpwAbC1.png" height="80%" width="80%" alt="kali ssh status inactive"/>

<p>Notice: ssh is inactive (dead) on this system</p>

<p>Enter the following command to start ssh</p>
<ul>
  <li>sudo systemctl start ssh</li>
</ul>

<img src="https://i.imgur.com/n8TaUKn.png" height="80%" width="80%" alt="kali ssh start"/>

<p>Verify that SSH is enabled again</p>
<ul>
  <li>sudo systemctl status ssh</li>
</ul>

<img src="https://i.imgur.com/pe5GDVL.png" height="80%" width="80%" alt="kali ssh status active"/>

<p>Notice that the output changed to active: active (running)</p>

<br />

<p>Open PuTTy and enter the IP address of the kali machine under Host Name or IP address</p>

<img src="https://i.imgur.com/DFWk4gf.png" height="80%" width="80%" alt="putty application gui"/>

<p>A prompt will pop up asking if you trust the connection, click accept or connect once to accept the connection or cancel if the machine is not trusted.</p>

<img src="https://i.imgur.com/I9qFNxH.png" height="80%" width="80%" alt="putty security alert"/>

<p>After accepting the security alert, login to the terminal with the kali credentials</p>

<img src="https://i.imgur.com/qXQ65KL.png" height="80%" width="80%" alt="putty terminal login"/>

<img src="https://i.imgur.com/YPLQxDk.png" height="80%" width="80%" alt="putty terminal login 2"/>

<p>Enter the following command to see what user you are logged in as</p>
<ul>
  <li>whoami</li>
</ul>

<img src="https://i.imgur.com/53mwz9d.png" height="80%" width="80%" alt="putty whoami"/>

<h3>Part 3: Create a group with Linux using PuTTy
</h3>

<p>Open terminal window in Kali Linux</p>

<br />

<p>To create a group enter the following command</p>
<ul>
  <li>sudo groupadd hq_admins</li>
  <li>Enter kali for the password if this is the first time you have used elevated privileges since loading up the machine</li>
</ul>

<img src="https://i.imgur.com/MF0FCYD.png" height="80%" width="80%" alt="sudo groupadd hq_admins"/>

<p>To verify the group has been made and added to the group directory</p>
<ul>
  <li>tail group add hq_admins</li>
</ul>

<img src="https://i.imgur.com/B2goTKO.png" height="80%" width="80%" alt="tail etc group"/>

<p>Notice the new group hq_admins has been added to the bottom of the file</p>

<h3>Part 4: Create a new user in the group hq_admins
</h3>

<p>Enter the following command to add a new user “ufour” to the hq_admins group as you create the account</p>
<ul>
  <li>sudo useradd ufour -m -g hq_admins</li>
  <li>-m (create home directory)</li>
  <li>-g (primary group)</li>
</ul>

<img src="https://i.imgur.com/zEIN8vB.png" height="80%" width="80%" alt="useradd ufour"/>

<p>Create a password for ufour</p>
<ul>
  <li>sudo passwd ufour</li>
  <li>For the lab environment kali was used as the password</li>
  <li>For production environments, use organization password policies</li>
</ul>

<br />

<p>Enter the following command to view the new user in the users file</p>
<ul>
  <li>sudo tail /etc/passwd</li>
</ul>

<img src="https://i.imgur.com/nJitVFq.png" height="80%" width="80%" alt="tail etc passwd"/>

<p>Notice at the bottom ufour is now listed</p>

<p>To view the password hash created for ufour enter the following command</p>
<ul>
  <li>sudo tail /etc/shadow</li>
</ul>

<img src="https://i.imgur.com/upLZOVu.png" height="80%" width="80%" alt="tail etc shadow"/>

<p>Enter the following commands to view the groups ufour is listed under</p>
<ul>
  <li>groups ufour</li>
</ul>

<img src="https://i.imgur.com/0WGWPRr.png" height="80%" width="80%" alt="groups ufour"/>

<p>To log in as the new ufour user</p>
<ul>
  <li>su - ufour</li>
  <li>Dash means a full login, any login scripts, ect.</li>
</ul>

<img src="https://i.imgur.com/AmIhcM6.png" height="80%" width="80%" alt="su - ufour"/>



<h3>Summary</h3>
<p>This project involves using PuTTY, an SSH client, to remotely log into a Linux system from a Windows machine. Once connected, create a new user group and a user account within that group on the Linux system. The demonstration ensures secure user management by assigning the appropriate group to the user on a Linux system.</p>
 
