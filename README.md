# Import required modules/packages/library
import pexpect

# Define variables
ip_address = '192.168.56.101'
username = 'prne'
password = 'cisco123!'
password_enable = 'class123!'
new_admin_username = 'Haroon'
new_admin_password = 'yourpassword'

# Create the SSH session
session = pexpect.spawn(f'ssh {username}@{ip_address}', encoding='utf-8', timeout=20)
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF])

# Check for error, if exists then display error and exit
if result != 0:
    print('--- FAILURE! creating a session for: ', ip_address)
    exit()

# Session expecting password, enter details
session.sendline(password)
result = session.expect(['>', pexpect.TIMEOUT, pexpect.EOF])

# Enter enable mode
session.sendline('enable')
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF])    

# Send enable password details
session.sendline(password_enable)
result = session.expect(['#', pexpect.TIMEOUT, pexpect.EOF])

# Enter configuration mode
session.sendline('configure terminal')
result = session.expect([r'.\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Change the hostname to Haroon
session.sendline('hostname Haroon')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Adding username and password new admin
session.sendline(f'username {new_admin_username} privilege 15 secret {new_admin_password}')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Adding a domain name
session.sendline('ip domain-name mynetwork.local')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Generating a crypto key for modules 2048
session.sendline('crypto key generate rsa modulus 2048')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Include ssh versions
session.sendline('ip ssh version 2')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Enter console line mode
session.sendline('line vty 0 4')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Add a transport for telnet and ssh
session.sendline('transport input telnet ssh')
result = session.expect([r'Haroon\(config-line\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Add a login local
session.sendline('login local')
result = session.expect([r'Haroon\(config-line\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Exit the console line mode
session.sendline('exit')

# Add security for https
session.sendline('ip http server')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Add server for http
session.sendline('ip http secure-server')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Add authentication local for https
session.sendline('ip http authentication local')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Enter access list mode
session.sendline('ip access-list extended IoT_Security')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Allow Ping on access list
session.sendline('permit icmp 192.168.56.0 0.0.0.255 any')
result = session.expect([r'Haroon\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Allow SSH on access list
session.sendline('permit tcp 192.168.56.0 0.0.0.255 any eq 22')
result = session.expect([r'Haroon\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Allow Telnet on access list
session.sendline('permit tcp 192.168.56.0 0.0.0.255 any eq 23')
result = session.expect([r'Haroon\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Allow HTTP on access list
session.sendline('permit tcp 192.168.56.0 0.0.0.255 any eq 80')
result = session.expect([r'Haroon\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Allow HTTPs on access list
session.sendline('permit tcp 192.168.56.0 0.0.0.255 any eq 443')
result = session.expect([r'Haroon\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Block anything else on access list
session.sendline('deny ip any any log')
result = session.expect([r'Haroon\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Exit access list mode
session.sendline('exit')

# Enter interface gigabit mode
session.sendline('int gig1')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Add the access list on the interface
session.sendline('ip access-group IoT_Security in')
result = session.expect([r'Haroon\(config-if\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Exit interface gigabit mode
session.sendline('exit')

# Save encryption password
session.sendline('service password-encryption')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Enter logging mode
session.sendline('logging buffered 4096 debugging')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Enable logging console
session.sendline('logging console')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Adding a logging host
session.sendline('logging host 192.168.56.200')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Enable logging trap information 
session.sendline('logging trap informational')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Enable logging trap warnings
session.sendline('logging console')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Entering buffered 64000
session.sendline('logging console')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Add timestamps logging time
session.sendline('service timestamps log datetime msec')
result = session.expect([r'Haroon\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

# Exit config mode
session.sendline('exit')

# Entering 1st Alert
session.sendline('send log "ALERT: SSH Login Attempt Detected on CSR1000v'

# 2nd Alert on Username
session.sendline('show running-config | include username')
result = session.expect[(r'Haroon\)#, pexpect.TIMEOUT, pexpect.EOF])

# Check for an error, alerting admin
if 'username' not in session.before:
    session.sendline('send log "ALERT: Unauthorised Admin Modification Detected!"')

# 3rd Alert on Ip address
session.sendline('show access-list IoT_Security')
result = session.expect[(r'Haroon\)#, pexpect.TIMEOUT, pexpect.EOF])

# Check for an error, alerting modification
if 'deny ip any any' not in session.before:
    session.sendline('send log "ALERT: ACL Modification Detected!"')

# Exit enable mode
session.sendline('exit')

# Display Success Message
print('------------------------------------------------------------------')
print('--- Success! Connected to: ', ip_address)
print('--- Hostname "Haroon" applied')
print('--- Secure telnet SSH configured')
print('--- TLS for IoT communication enabled')
print('--- ACL "IoT_Security" applied')
print('--- Logging and Monitoring enabled')
print('------------------------------------------------------------------')
