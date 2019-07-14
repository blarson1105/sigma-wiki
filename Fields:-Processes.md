| Field | Type | Description |
|----------------------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| process_action | string | Endpoint solution's action on process (e.g. 'allow', 'block') |
| process_authentihash | string | Process image Authentihash (e.g. 'cfc5937b4db3a1d5718fe144b621d50b0a337854ab3f5e2df152b62627f6fd4a') |
| process_authentihash | string | Process image Authentihash (e.g. 'cfc5937b4db3a1d5718fe144b621d50b0a337854ab3f5e2df152b62627f6fd4a') |
| process_cmd | string | Full process command line (e.g. 'ssh -l user 10.0.0.16') |
| process_cpu_load | float | CPU load consumed by the process (in percent) (e.g. '98.5') |
| process_cwd | string | The process' working directory (e.g. 'C:\temp') |
| process_image | string | Process executable (e.g. 'C:\Windows\System32\calc.exe') |
| process_image_company | string | Process image file copyright (e.g. 'Microsoft Corporation') |
| process_image_description | string | Process image file description (e.g. 'Microsoft Management Console') |
| process_image_file_version | string | Process image file version (e.g. '6.1.7600.16385 (win7_rtm.090713-1255)') |
| process_image_product | string | Process image file product (e.g. 'Microsoft® Windows® Operating System') |
| process_imphash | string | Process image Imphash (e.g. 'ceefb55f764020cc5c5f8f23349ab163') |
| process_imphash | string | Process image Imphash (e.g. 'ceefb55f764020cc5c5f8f23349ab163') |
| process_integrity | string | The Windows integrity level of the process (system, high, medium, low, untrusted) |
| process_is_hidden | boolean | Is the process hidden or not (e.g. false) |
| process_is_rare | boolean | Is the process rare (anomaly detection) (e.g. false) |
| process_logon_guid | string | Process logon GUID (e.g. '{c1b49677-9fde-5d1b-0000-002064d30200}') |
| process_md5 | string | Process image MD5 hash (e.g. 'ad7b9c14083b52bc532fba5948342b98') |
| process_mem_used | int | Memory used by process (working set size in byte) (e.g. 120450000)  |
| process_name | string | Process name (e.g. 'ssh') |
| process_os | string | Operating system (full os information, compare Python's platform.uname()) |
| process_parent_cmd | string | Full parent process command line (e.g. 'sshd') |
| process_parent_cwd | string | The parent process' working directory (e.g. 'C:\Windows\System32') |
| process_parent_image | string | Parent process executable (e.g. 'C:\Windows\System32\svchost.exe') |
| process_parent_md5 | string | Process image MD5 hash (e.g. 'ad7b9c14083b52bc532fba5948342b98') |
| process_parent_name | string | Parent process name (e.g. 'svchost.exe') |
| process_parent_path | string | Parent process image location (e.g. 'C:\Windows') |
| process_parent_sha1 | string | Process image SHA1 hash (e.g. 'ee8cbf12d87c4d388f09b4f69bed2e91682920b5') |
| process_parent_sha256 | string | Process image SHA256 hash (e.g. '17f746d82695fa9b35493b41859d39d786d32b23a9d2e00f4011dec7a02402ae') |
| process_path | string | Process image location (e.g. '\\tsclient\temp') |
| process_pgid | string | Identifier of the group of processes the process belongs to |
| process_pid | long | Process PID (e.g. 1337) |
| process_ppid | long | Parent Process PID (e.g. 42) |
| process_priority | int | Nice level (Linux, Unix) and process priority (Windows; 'Realtime' = -20, 'High' = -15, 'Above normal' = -5, 'Normal' = 0, 'Below normal' = 10, 'Low' = 19) (e.g. -10) |
| process_sha1 | string | Process image SHA1 hash (e.g. 'ee8cbf12d87c4d388f09b4f69bed2e91682920b5') |
| process_sha256 | string | Process image SHA256 hash (e.g. '17f746d82695fa9b35493b41859d39d786d32b23a9d2e00f4011dec7a02402ae') |
| process_start | datetime | The time the process started (e.g. '2016-05-23T08:05:34.853Z') |
| process_temin_sess_id | string | Process terminal session ID (e.g. 1) |
| process_threadid | long | Thread ID (e.g. 4242) |
| process_title | string | Process title. The proctitle, sometimes the same as process name. Can also be different: for example a browser setting its title to the web page currently opened. |
| process_uid | string | User id of the user that created the process (e.g. '1', 'S-1-5-21-549688327-91903405-2500298261-1000') |
| process_uptime | long | Seconds the process has been up (e.g. 127381) |
| process_user | string | User account reference that created the process (e.g. 'Administrator', 'DOM\u123') |