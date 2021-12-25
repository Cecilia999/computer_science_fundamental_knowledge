# Application Layer

# Application-Layer Protocols & Architecture

## Application-Layer Architecture

### 1. client-server architecture

### 2. peer-to-peer architecture

## Application-Layer Protocols

1. TELNET:

   Telnet stands for the **TELetype NETwork**. It allows Telnet clients to access the resources of the Telnet server. It is used for managing files on the internet. It is used for the initial setup of devices like switches. The telnet command is a command that uses the Telnet protocol to communicate with a remote device or system. **Port number of telnet is 23**

   ```
   telnet [\\RemoteServer]
   \\RemoteServer : Specifies the name of the server to which you want to connect
   ```

2. FTP(file transfer protocol):

   It is the protocol that actually lets us transfer files. It can facilitate this between any two machines using it. But FTP is not just a protocol but it is also a program.FTP promotes sharing of files via remote computers with reliable and efficient data transfer. **The Port number for FTP is 20 for data and 21 for control.**
   `ftp machinename`

3. NFS(network file system):

   It allows remote hosts to mount file systems over a network and interact with those file systems as though they are mounted locally. This enables system administrators to consolidate 合并 resources onto centralized servers on the network. The Port number for NFS is 2049.

   **Mounting** is a process by which the operating system makes files and directories on a storage device available for users to access via the computer's file system. **---->** Before a user can access a file on a Unix-like machine, the file system on the device which contains the file needs to be mounted with the mount command.

   `service nfs start`

4. SMTP(Simple Mail Transfer Protocol):

   It is a part of the TCP/IP protocol. Using a process called “store and forward,” SMTP moves your email on and across networks. It works closely with something called the Mail Transfer Agent (MTA) to send your communication to the right computer and email inbox. The Port number for SMTP is 25.

5. SNMP(Simple Network Management Protocol):

   It stands for Simple Network Management Protocol. It gathers data by polling the devices on the network from a management station at fixed or random intervals, requiring them to disclose certain information. It is a way that servers can share information about their current state, and also a channel through which an administrate can modify pre-defined values. The Port number of SNMP is 161(TCP) and 162(UDP).

6. DNS(Domain Name System):

   Every time you use a domain name, therefore, a DNS service must translate the name into the corresponding IP address. The Port number for DNS is 53.

7. DHCP(Dynamic Host Configuration Protocol):

   It gives IP addresses to hosts. There is a lot of information a DHCP server can provide to a host when the host is registering for an IP address with the DHCP server. Port number for DHCP is 67, 68.

# The Web, HTTP and HTTPS

## 1. [HTTP](http.md)

## 2. [HTTPS](https.md)

## 3. [cookie/session/token](cookie_session_token.md)

# [DNS](DNS.md)

# Video Streaming and Content Distribution Networks

```

```
