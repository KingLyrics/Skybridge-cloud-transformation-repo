#  Phase one synopsis
    I am a cloud engineer assigned to a client onboarding project
    Objective: Build their core infrastructure from scratch so it can later be migrated to the cloud.

# Structure
`Company: SkyBridge Logistics`

`Network: 192.168.10.0/24`

Servers:
- DC01 → Domain Controller (AD DS, DNS)
- FS01 → Linux File/App Server
- CL01 → Employee Workstation

`Domain: skybridge.local`

## Windows Server Configuration
| Setting | Value               |
| ------- | ------------------- |
| Name    | DC01                |
| OS      | Windows Server 2025 |
| RAM     | 2 GB                |
| CPU     | 2 cores             |
| Disk    | 50 GB               |
| Network | Host-only           |

`IP: 192.168.10.10`
`Subnet: 255.255.255.0`
`DNS: 192.168.10.10`
`Gateway: (blank)`
- Attached second network adapter in NAT configuration to allow connection to the internet.