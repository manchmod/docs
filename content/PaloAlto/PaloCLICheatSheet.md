---
title: "Palo Cheat Sheet"
date: 2019-10-27T12:32:02-04:00
draft: false
---

Show general system health information.

` show system info`

Show percent usage of disk partitions.

` show system disk-space`

Show the maximum log file size.

` show system logdb-quota`

Show running processes.

` show system software status`

Show processes running in the management plane.

` show system resources`

Show resource utilization in the dataplane.

` show running resource-monitor`

Show the licenses installed on the device.

` request license info`

Show when commits, downloads, and/or upgrades are completed.

` show jobs processed`

Show session information.

` show session info`

Show information about a specific session.

` show session id <session-id>`

Show the running security policy.

` show running security-policy`

Show the authentication logs.

` less mp-log authd.log`

Restart the device.

` request restart system`

Show the administrators who are currently logged in to the web interface, CLI, or API.

` show admins`

Show the administrators who can access the web interface, CLI, or API, regardless of whether those administrators are currently logged in. When you run this command on the firewall, the output includes both local administrators and those pushed from a Panorama template.

` show admins all`

Configure the management interface as a DHCP client. For a successful commit, you must include each of the parameters: accept-dhcp-domain, accept-dhcp-hostname, send-client-id, and send-hostname.

`set deviceconfig system type dhcp-client accept-dhcp-domain <yes|no> accept-dhcp-hostname <yes|no> send-client-id <yes|no> send-hostname <yes|no>`