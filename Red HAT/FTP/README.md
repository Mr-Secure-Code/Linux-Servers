# FTP (File Transfer Protocol)

File Transfer Protocol (FTP) is a standard network protocol used to transfer files from one host to another over a TCP-based network, such as the internet The default port for FTP servers is TCP port no 21.

# Configure FTP Server.

-   **FTP Client Tool.**

    *Install ftp client tool.*
    ```
    yum install ftp
    ```
    *Connect to an FTP server.*
    ```
    ftp ftp_connation_ip
    ```
-	**FTP Server Tool.**
	```
	yum install vsftpd
    ```

-   **Files.**

	*Main Configuration file.*	
	```
    /etc/vsftpd/vsftpd.conf
	```
	
	*The default location for publicly available files that can be accessed by anonymous users.*
    ```
	/var/ftp/pub
    ```

-   **Enable passive mode.**
    
    *Passive mode is a mode of operation in which the client initiates the data connection to the server rather than the server initiating the connection to the client.*
    ```
    vim /etc/vsftpd/vsftpd.conf
    ```
        
        pasv_enable=YES
        pasv_min_port=55000
        pasv_max_port=55999

-   **Allow Port In iptables**
    
    *Edit This File.*
	```
	vim /etc/sysconfig/iptables
	```
    ```
    -A INPUT -p tcp --dport 21 -j ACCEPT
	-A INPUT -p tcp --dport 55000:55999 -j ACCEPT
    ```
    *Start iptable service.*
    ```
    systemctl restart iptables.service
    ```
-   **Start the vsftpd service and enable it to start automatically at boot time.**
    ```
    systemctl start vsftpd
    ```
    ```
    systemctl enable vsftpd
    ```

    Default User :- anonymous,ftp

-   **Block Default User.**

    *Edit File*
    ```
    vim /etc/vsftpd/vsftpd.conf
    ```
    Search:- **anonymous_enable=YES** Convert with **anonymous_enable=NO**

-   **Unlock Default User.**

    *Edit File*
    ```
    vim /etc/vsftpd/vsftpd.conf
    ```
    Search:- **anonymous_enable=NO** Convert with **anonymous_enable=YES**

    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
-   **Unblock Regular user.**

    *Edit File*
    ```
    vim /etc/vsftpd/vsftpd.conf
    ```
    1.Search:- **chroot_local_user=YES** Uncommant this line.
    
    2.Add this line at the end. **allow_writeable_chroot=YES**
 
    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
-   **Unblock Root User.**

    *Commant root user in this files.*
    ```
    vim /etc/vsftpd/user_list
    ```
    ```
    vim /etc/vsftpd/ftpusers
    ```
    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
-   **Block Root User.**

    *Uncommant root user in this files.*
    ```
    vim /etc/vsftpd/user_list
    ```
    ```
    vim /etc/vsftpd/ftpusers
    ```

    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
-   **Change Default Directory.**

    1 Create a folder for users.
    ```
    mkdir /folder_name
    ```
    ```
    vim /etc/vsftpd/vsftpd.conf
    ```
    
    Add This line.

        local_root=/folder_name

    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
-   **Deny list.**

    *A Deny list is a list of  users that are not allowed to access the ftp server.*

    **Files.**
    ```
    vim /etc/vsftpd/user_list
    ```
    ```
    vim /etc/vsftpd/ftpusers
    ```

    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
-   **Allow list.**

    *Make Allow user file.*
    ```
    vim /etc/vsftpd/allow_users
    ```
    Note:- Entry Allow User in this file.

    *After allow user entry edit main configuration file.*
    
    1.Search:- **chroot_local_user=YES** commant this line.
    ```
    vim /etc/vsftpd/vsftpd.conf
    ```
    2.Add This Line.
    ```
    userlist_enable=YES
    userlist_file=/etc/vsftpd/allow_users
    userlist_deny=NO
    ```
    *restart service*
    ```
    systemctl restart vsftpd.service
    ```
