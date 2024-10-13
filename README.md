<body>
    <h1>Setting Up a Home Lab with Active Directory in VirtualBox</h1>
    <p> This guide walks you through the detailed steps for setting up a basic Active Directory (AD) environment in a home lab using Oracle VirtualBox. </p>
    <h2>Prerequisites</h2>
<ul>
    <li>Hardware: At least 8 GB RAM, multi-core CPU, 50 GB of storage.</li>
    <li>Software: Oracle VirtualBox, Windows Server ISO.</li>
    <li>Licenses for Windows Server (evaluation versions can be used).</li>
</ul>

<h2>Step 1: Download and Install Oracle VirtualBox</h2>
<ol>
    <li>Go to the <a href="https://www.virtualbox.org/wiki/Downloads" target="_blank">Oracle VirtualBox website</a> and download the installer.</li>
    <li>Follow the prompts to install VirtualBox, including network interfaces.</li>
    <li>(Optional) Install the VirtualBox Extension Pack to enable additional features.</li>
</ol>

<h2>Step 2: Create a Virtual Machine and Install Windows Server</h2>
<ol>
    <li>Create a new virtual machine (VM) in VirtualBox:
        <ul>
            <li>Name: "Windows Server AD".</li>
            <li>Type: Microsoft Windows, Version: Windows Server (64-bit).</li>
            <li>Allocate 4 GB of RAM and create a dynamically allocated virtual hard disk (50 GB).</li>
        </ul>
    </li>
    <li>In VM settings, attach the Windows Server ISO under "Storage".</li>
    <li>Start the VM and follow the installation steps for Windows Server, setting up an administrator account and a static IP address.</li>
</ol>

<h2>Step 3: Install Active Directory Domain Services (AD DS)</h2>
<ol>
    <li>Open "Server Manager" and click on "Add Roles and Features".</li>
    <li>Select "Active Directory Domain Services" and add the required features.</li>
    <li>Once the installation completes, click "Promote this server to a domain controller".</li>
    <li>Create a new forest and specify the root domain name (e.g., "home.lab").</li>
</ol>

<h2>Step 4: Configure DNS</h2>
<ol>
    <li>Verify that DNS is installed during AD DS setup.</li>
    <li>Open "DNS Manager" and configure forwarders (e.g., 8.8.8.8 for Google DNS).</li>
    <li>Create DNS zones if necessary and test DNS resolution using <code>nslookup</code>.</li>
</ol>

<h2>Step 5: Verify Domain Controller Setup</h2>
<ol>
    <li>Open "Active Directory Users and Computers" to confirm the domain structure is visible.</li>
    <li>Run <code>dcdiag</code> and <code>repadmin</code> to ensure AD health and replication are functioning properly.</li>
</ol>

<h2>Step 6: Manage Users with PowerShell</h2>
<ol>
    <li>Open PowerShell as Administrator and import the Active Directory module:
        <pre><code>Import-Module ActiveDirectory</code></pre>
    </li>
    <li>Create a new user:
        <pre><code>New-ADUser -Name "John Doe" -SamAccountName "jdoe" -UserPrincipalName "jdoe@home.lab" -Path "OU=Users,DC=home,DC=lab" -AccountPassword (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force) -Enabled $true</code></pre>
    </li>
    <li>Bulk user creation:
        <pre><code>Import-Csv -Path "C:\Path\To\users.csv" | ForEach-Object { New-ADUser -Name "$($_.FirstName) $($_.LastName)" -GivenName $_.FirstName -Surname $_.LastName -SamAccountName $_.SamAccountName -UserPrincipalName "$($_.SamAccountName)@home.lab" -Path $_.OU -AccountPassword (ConvertTo-SecureString $_.Password -AsPlainText -Force) -Enabled $true }</code></pre>
    </li>
    <li>Manage user accounts (enable, disable, delete, move users, etc.).</li>
</ol>

<h2>Additional Steps</h2>
<ul>
    <li><strong>Organizational Units (OUs):</strong> Create OUs for organizing users and delegate control.</li>
    <li><strong>Group Policies:</strong> Enforce security policies via Group Policy Management.</li>
    <li><strong>Multiple Domain Controllers:</strong> Set up secondary domain controllers for redundancy.</li>
    <li><strong>DHCP Server:</strong> Install and configure a DHCP server if needed for dynamic IP addressing.</li>
    <li><strong>Backup and Recovery:</strong> Set up regular backups using Windows Server Backup.</li>
    <li><strong>Monitoring:</strong> Use tools like <code>dcdiag</code> and third-party tools to monitor AD health.</li>
</ul>

<h2>Troubleshooting</h2>
<ul>
    <li><strong>VM Boot Issues:</strong> Check the boot order and ensure the ISO is correctly attached.</li>
    <li><strong>Network Issues:</strong> Verify that the VM's network adapter is configured correctly.</li>
    <li><strong>DNS Failures:</strong> Ensure the DNS server is properly configured and clients point to it.</li>
    <li><strong>Replication Problems:</strong> Run <code>repadmin /showrepl</code> to diagnose replication issues.</li>
</ul>

<h2>Conclusion</h2>
<p>By following these detailed steps, you can successfully set up a home lab with Active Directory using VirtualBox. This hands-on lab provides valuable experience with server administration, user management, DNS configuration, and AD DS setup. Experiment with additional features and best practices to further expand your skills.</p>
