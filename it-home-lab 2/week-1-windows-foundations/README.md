# Week 1: Windows Foundations

Lab machine: **win-lab01**, a Windows 11 virtual machine in VMware Fusion.

## 1. Device Manager

- **⚠ Warning sign on a device:** the driver is missing or faulty. As helpdesk, my first step is to update or reinstall the driver.

## 2. Core commands (Win + R)

| Command | Opens |
|---|---|
| `lusrmgr.msc` | Local Users and Groups |
| `devmgmt.msc` | Device Manager |
| `diskmgmt.msc` | Disk Management |
| `eventvwr.msc` | Event Viewer |
| `services.msc` | Services (for example Print Spooler, DHCP Client) |
| `msconfig` | System Configuration |
| `compmgmt.msc` | Computer Management |

- If a PC has an IP address problem: `ipconfig /release` then `ipconfig /renew`
- In `msconfig` → Services, tick **Hide all Microsoft services** to see only third-party services running in the background.

## 3. VMware Tools drivers

| Driver | What it fixes |
|---|---|
| vmxnet | Network adapter, IP and DHCP |
| SVGA | Display problems |
| vmusbmouse | Smooth mouse movement |
| Tools service | Drag and drop between the Mac and the VM |

## 4. `ipconfig /all`

- **IPv4 Address:** confirms the PC has a valid IP
- **Subnet Mask:** confirms the PC is on the right network
- **Default Gateway:** needed to reach the internet, even with a valid IP
- **DNS Servers:** if these are wrong, websites won't load
- **Physical Address (MAC):** identifies each device on the network
- **DHCP Enabled:** Yes = set automatically. No = someone typed the IP in by hand.

## 5. Ping

- `ping 192.168.1.1` asks the router "are you there?"

## 6. Basic network troubleshooting

| Command | Question it answers |
|---|---|
| `tracert` | Where does my connection break on the way there? |
| `nslookup` | Can DNS turn this website name into an address? |
| `ping google.com` | Uses whichever IP version DNS returns first (often IPv6 if available) |
| `ping -4 google.com` | Forces the ping to use IPv4 only |

## 7. The three main Event Viewer logs

| Log | What it records |
|---|---|
| Application | Events from installed apps: crashes, install errors, app problems |
| Security | Logins (successful and failed), account lockouts, permission changes, audit events |
| System | Windows itself: driver failures, service crashes, hardware errors, boot issues |

## 8. Security issues in Event Viewer

Event ID **4625** flags a failed logon in the Security log. I filter for it directly. Many 4625s from the same source in a short time is a key sign of a brute-force attack.

## 9. System issues in Event Viewer

I check the **System** log, sort by level to find Errors and Warnings, then note the **Source** and **Event ID**. The Event ID can be searched, and it often leads straight to a known fix from Microsoft or the community.

## 10. Uninstalling apps

| Method | What it is |
|---|---|
| Settings → Apps | The modern Windows 11 screen. Works for normal apps and Microsoft Store apps. |
| Control Panel → Programs and Features | The classic screen. Only shows normal desktop apps, not Store apps. |

## 11. Using services.msc

I use `services.msc` to find, stop and restart services. Restarting the **Print Spooler** is one of the fastest fixes for stuck print jobs. I also check the **Startup Type**:

- **Automatic:** starts every time Windows boots
- **Manual:** only starts when something asks for it
- **Disabled:** never starts. Check this if a service keeps failing after a restart.

## 12. Startup apps

Task Manager → **Startup apps** → enable or disable each one.

## 13. Troubleshooting a PC that crashes randomly

I do a clean boot with `msconfig`: turn off third-party services and startup items, test after each restart, and narrow it down to the exact cause before changing anything else.

## 14. Opening a shared folder

`\\WIN-LAB01\Shared`

Two backslashes, then the computer name, then a backslash, then the folder name.

## 15. NTFS folder permissions

Right-click the folder → Properties → Security.

| Permission | What it allows |
|---|---|
| Full Control | Read, write, delete and change permissions |
| Modify | Read, write and delete files, but not change permissions |
| Read & Execute | Open and run files, but not change anything |
| Read | Open files only (most restrictive) |
| Write | Create new files, but not read existing ones |

## 16. NTFS vs Share permissions

- **Share permissions:** only apply over the network, have 3 basic levels, and are ignored when someone signs in locally.
- **NTFS permissions:** apply everywhere (locally and over the network), are more detailed, and are always enforced.
- **When both apply, the stricter one wins.**
- **Best practice:** set the share to Full Control and control access with NTFS.

## Week 1 summary

In week 1 I practised the basics of Windows administration: managing users with `lusrmgr.msc`, checking networks with `ipconfig`, `ping`, `tracert` and `nslookup`, reading event logs (including filtering for Event ID 4625 as a brute-force sign), managing services with `services.msc` and `msconfig`, and sharing folders with NTFS and Share permissions. I also fixed real problems along the way, including missing VMware drivers, an IPv6 routing fault on my home internet connection and permission conflicts. Each time I worked from the symptoms to the cause instead of guessing.
