I was trying getting data from REST services in APEX and was facing multiple issues. After lot of search on google I was able to resolve all the issues but unfortuantely there was no single resource where I got solutions to all the problems so I decided to put together it here.
All of the web pages I used are mentioned at the end of this article. thank you allto the original authors of those posts.

# Issue 1 - ORA-24247: network access denied by access control list (ACL)

## Add target host to ACL (Access Control Lists)

To start with you need to create something called ACL to avoid the error **network access denied by access control list** which means no outbound calls are allowed to this host.

There are 2 steps to this.
1. Create ACL
2. Add 

```
BEGIN
  DBMS_NETWORK_ACL_ADMIN.create_acl (
    acl          => 'earthquake3.xml', 
    description  => 'ACL for geojson',
    principal    => 'APEX_210100',
    is_grant     => TRUE, 
    privilege    => 'connect',
    end_date     => NULL);

  COMMIT;
END;

BEGIN
  DBMS_NETWORK_ACL_ADMIN.assign_acl (
    acl         => 'earthquake3.xml',
    host        => '*.usgs.gov', 
    lower_port  => NULL,
    upper_port  => NULL);
COMMIT;
END;
```

## A. Create an Oracle wallet.

Create a folder wallet to hold the certificate under the ORACLE_HOME directory. 

Tip for windows - on windows to find the oracle home , find the parent folder of bin folder, the bin folder is where sqlplus.exe resides. 

```
cd $ORACLE_HOME  
mkdir wallet  
cd wallet  
orapki wallet create -wallet . -pwd Welcome1 -auto_login
```

## Get the target host’s root certificate

This usually should not be a problem , go to browser and download it
but there are possible problems here too. I would add more details here.

## C. Add certificate in the wallet

Go to the wallet location, and move the downloaded certificate here, and add it to the wallet. 
if your file name is certificate.cer
```
cd $ORACLE_HOME/wallet  
orapki wallet add -wallet . -trusted_cert -cert certificate.cer -pwd Welcome1  
```
orapki wallet display -wallet 
-- Note - I did not understand the output by this command.
 
 


# Issue 3 - ORA-28759: failure to open file.

if you see this error means - you need to add privileges for the appropriate user or group for your 2 files in wallet folder. Privileges may not be present if you have used orakpi utility.
1. wallet.sso 
2. ewallet.p12 

For Windows,
1.Select the file, right-click, and select Properties.
2.Select the Security tab and click Change Permissions.
3.Click Add > Locations, and select the appropriate location.
4.In the Select User or Group field, type ora_dba.Click the Check names button to verify that the ora_dba group exists.
5.Click OK and the Permission Entry dialog box displays.
6.Select the Allow check box next to Full Control and click OK.
7.In the Advanced Security dialog box click Apply.
8.Click OK to exit the dialogs.

this is as per original post but in my case ora_dba group was not there 
I added this user in the permissions 
SSAPRE-SURFACE\ORA_OraDB18Home1_SVCACCTS

how I got this user ? 
A - This was in the permissions for the wallet folder !  so i tried it and it worked. 


References
https://www.globalvoxinc.com/call-apis-from-oracle-database/
https://stackoverflow.com/questions/26697841/oracle-error-ora-28759-failure-to-open-file-when-requesting-utl-http-package

