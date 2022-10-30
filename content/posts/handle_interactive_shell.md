---
title: "Handle Interactive Shell"
date: 2022-10-30T09:50:26+08:00
draft: false
categories: ["blog"]
tags: ["blog", "ssh", "paramiko", "python"]
---

## Introduction

Connecting to a remote service using a python module **paramiko** has always been one of the well-known methods to handle such tasks. Executing commands and receiving the data from the connected server can be done quickly. The following code is a primary usage of **paramiko** to interact with the server.

```python
import paramiko

host = '127.0.0.1' # server host
username = 'root'
password = 'root'

# Init SSH client
client = paramiko.client.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
client.connect(host, username=username, password=password)

# Send command
_stdin, _stdout,_stderr = client.exec_command('pwd')
print(_stdout.read().decode())

client.close()
```

```shell
/c/Users/n.arunoprayoch
```

However, when it comes to connecting to a Cisco device via SSH, it is not that simple. This is a case study of the Cisco CUCM Cluster SSH connection. When an SSH connection is established on a Cisco CUCM cluster, it activates an interactive session depicted in the following image

{{< image src="https://i.ibb.co/PgkVBh1/download.png" alt="Cisco CUCM Cluster CLI" position="center">}}

If we were to connect and retrieve the data using **exec_command** method, we would only keep getting this result

```shell
Command Line Interface is starting up, please wait ..
```

## Solution

1. Connect to a Cisco CUCM Cluster device with SSH using **paramiko**
2. Initiate SSH channel by using **ssh_client.invoke_shell()**
3. Create an infinite loop (with termination conditions) to retrieve and send data

{{< image src="https://i.ibb.co/RbBNv74/cucm-attemp-flowchart-attemp-2.jpg" alt="flowchart" position="center">}}

According to the diagram above, we can write code as follows:

```python
import paramiko


class SSHService:
    def __init__(self, ssh_hostname, ssh_username, ssh_password, ssh_port, ssh_timeout):
        # SSH client
        ssh = SSHClient()
        ssh.set_missing_host_key_policy(AutoAddPolicy())

        # Create SSH client connection
        ssh.connect(hostname=ssh_hostname, username=ssh_username,
                    password=ssh_password, port=ssh_port, timeout=ssh_timeout)

        # Add a delay to wait for finishing logging-in
        if apply_conn_delay:
            time.sleep(SSH_CONN_DELAY)

        # Assign SSH to attrs
        self.ssh_client = ssh
        self.ssh_channel = ssh.invoke_shell() if invoke_shell else None


class CUCMClusterService:
    def __init__(self):
        self.shell_service = ShellService(ssh_hostname, ssh_username, ssh_password, ssh_port, ssh_timeout)
    
    def get_cucm_cluster_data(self):
        # All data
        channel_data = ''
        cmd_data = ''

        # Memento
        prev_data = ''
        cur_data = ''

        # States
        retry_num = 3
        cmd_sent = False

        # Example of command queue
        command_queue_list = deque([
            'utils disaster_recovery history backup\r',
        ])

        while True:
            # Keep receiving the data from SSH channel
            if not retry_num:
                print('Out of Retries. Break the channel.')
                break

            # Check if shell is ready to receive data
            if self.shell_service.recv_ready():
                # Get data
                cur_data = self.shell_service.recv_ssh_data_from_channel()

                # Check duplication between cur_data and prev_data
                # If they are the same, assume that the system is idle; break the loop
                if prev_data and cur_data == prev_data:
                    print('Duplicate Recv found. Break the channel.')
                    break

                # Update data
                prev_data = cur_data
                channel_data += cur_data
                if cmd_sent:
                    cmd_data += cur_data

                # Reset states
                retry_num = 3
                cmd_sent = False
            else:
                print('Recv Not Ready: {}.'.format(retry_num))
                retry_num -= 1
                continue

            # Wait for SSH_CUCM_PROMPT prompt to start sending cmd(s)
            if 'admin:' in cur_data and command_queue_list:
                cmd = command_queue_list.popleft()
                self.shell_service.send_ssh_cmd_to_channel(cmd)
                cmd_sent = True

        return str(cmd_data)
```

Essentially, we can successfully establish the connection, send commands, and retrieve the data from the connected CUCM cluster.

## Key Points

1. Investigation: SSH type (ordinary, interactive, etc.)
2. Regarding **paramiko** lib, use **.invoke_shell()** method to establish an SSH channel to handle an interactive CLI
3. Create an infinite loop to interact with the established channel
4. Ensure that the loop has termination conditions setup
    - Max Loop
    - Max duplicate result
    - Max time
    - No command(s) in queue
    - Etc.
5. Add a delay for each interaction to let the system finish its process
