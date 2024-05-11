# Here is a Standard best practice Cisco Switch configuration :computer:

## Basic switch configuration :bulb:
enable
configure terminal

## Set the hostname for identification
hostname Switch24Port

## Secure access by encrypting plain text passwords
service password-encryption

## Disable unused services
no ip http server
no ip http secure-server
no cdp run

## Configure console and vty lines for secure management
line console 0
  logging synchronous
  login local
  exec-timeout 10 0
  exit

line vty 0 15
  transport input ssh
  login local
  exec-timeout 10 0
  exit

## Enable SSH and set domain for crypto key generation
ip domain-name local.net
crypto key generate rsa modulus 2048

## Configure a local username and password for device access :rocket:
username admin privilege 15 secret 0 StrongPassword

## Create and configure the management VLAN
vlan 99
  name Management
  exit

interface vlan 99
  ip address 192.168.99.2 255.255.255.0
  no shutdown
  exit

## Set the default gateway
ip default-gateway 192.168.99.1

## Configure all interfaces
interface range FastEthernet0/1-24

  ### Enable port security
  switchport mode access
  switchport port-security
  switchport port-security maximum 2
  switchport port-security violation restrict
  switchport port-security aging time 2
  switchport port-security aging type inactivity

 ### Specify MAC addresses if known, else sticky to learn dynamically
  switchport port-security mac-address sticky

  ### Disable DTP
  switchport nonegotiate

  ### Enable BPDU Guard on all access ports to prevent potential STP attacks
  spanning-tree bpduguard enable

  ### Set the port to access VLAN (example VLAN 10 for users)
  switchport access vlan 10

  ### Shutdown unused ports
  shutdown
  exit

## Enable logging :gear:
logging buffered 52000

## Set system time zone (example for Eastern Time)
clock timezone EST -5
clock summer-time EDT recurring

## Save the configuration
end
write memory

# Additional Notes :warning:

### Service password-encryption: 
Encrypts passwords in the configuration file to prevent easy viewing.
### Management VLAN Configuration: 
It’s a good practice to isolate management traffic on its VLAN.
### Secure Shell (SSH): 
Provides secure remote management by encrypting all traffic.
### Port Security: 
Limits the number of MAC addresses on a port and configures actions for security violations.
### BPDU Guard:
Protects against Spanning Tree Protocol (STP) manipulation attacks.
