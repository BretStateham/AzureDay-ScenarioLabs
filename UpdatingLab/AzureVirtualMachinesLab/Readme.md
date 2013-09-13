<a name="Title" />
#Moving a Local Web Site and SQL Database to Windows Azure Virtual Machines#

---

<a name="Overview">
##Overview##

In this lab, you will move a web site and database running on the local network (your machine) to Virtual Machines running in Windows Azure.  

You will configure two new Windows Azure Virtual Machines:

1. A Windows Server 2012 Server.  You will add IIS to this server and configure the firewall to allow the website to be served to the public from here.
2. A Windows Server 2012 Server with SQL Server 2012.  You will then migrate the database from the local server to the SQL Server instance running in that Virtual Machine.  You will also need to properly configure the firewall on the virtual machine to allow connections from the web server vm. 

Once the virtual machines have been configured you will configure the public endpoints to allow connections from outside the Windows Azure data center.




