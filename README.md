import pexpect

ip_address = '192.168.56.101'
username = 'prne'
password = 'cisco123!'
password_enable = 'class13!'
new_admin_username = 'Haroon'
new_admin_password = 'yourpassword'

session = pexpect.spawn(f'ssh {username]@{ip_address}', encoding='utf-8', timeout=20)
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF])

if result != 0:
    print('--- FAILURE! creating a session for: ', ip_address)
    exit()

session.sendline(password)
result = session.expect(['>', pexpect.TIMEOUT, pexpect.EOF])

session.sendline('enable')
result = session.expect(['Password:', pexpect.TIMEOUT, pexpect.EOF])    

session.sendline(password_enable)
result = session.expect(['#', pexpect.TIMEOUT, pexpect.EOF])

session.sendline('configure terminal')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('hostname Haroon')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline(f'username {new_admin_username} privilege 15 secret {new_admin_password})
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('ip domain-name mynetwork.local')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('crypto key generate rsa modules 2048')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('ip ssh version 2')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('line vty 0 4')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('transport input ssh')
result = session.expect([r'\(config-line\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('login local')
result = session.expect([r'\(config-line\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('exit')

session.sendline('ip http secure-server')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('ip http authentication local')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('ip acess-list extended IoT_Security')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('permit tcp 192.168.56.0 0.0.0.255 any eq 443')
result = session.expect([r'\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('deny ip any any log')
result = session.expect([r'\(config-ext-nac1\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('exit')

session.sendline('int gig1')
result = session.expect([r'\(config-if\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('ip access-group IoT_Security in')
result = session.expect([r'\(config-if\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('exit')

session.sendline('logging buffered 4096 debugging')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('logging console')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('logging trap informational')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('service timestamps log datetime msec')
result = session.expect([r'\(config\)#', pexpect.TIMEOUT, pexpect.EOF]) 

session.sendline('exit')

session.sendline('exit')

#Display Success Message
print('------------------------------------------------------------------')
print('--- Success! Connected to: ', ip_address)
print('--- Secure telnet configured')
print('--- TLS for IoT communication enabled')
print('--- ACL "IoT_Security" applied')
print('--- Logging and Monitoring enabled')
print('------------------------------------------------------------------')
