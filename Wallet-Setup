## A. Create an Oracle wallet.

Create a folder wallet to hold the certificate under the ORACLE_HOME directory. 

Tip for windows - on windows to find the oracle home , find the parent folder of bin folder, the bin folder is where sqlplus.exe resides. 


cd $ORACLE_HOME  
mkdir wallet  
cd wallet  
orapki wallet create -wallet . -pwd Welcome1 -auto_login


## Get the target host’s root certificate

This usually should not be a problem , go to browser and download it
but there are possible problems here too. I would add more details here.

## C. Add certificate in the wallet

Go to the wallet location, and move the downloaded certificate here, and add it to the wallet. 
if your file name is certificate.cer


cd $ORACLE_HOME/wallet  
orapki wallet add -wallet . -trusted_cert -cert certificate.cer -pwd Welcome1  
 orapki wallet display -wallet 
 usually you wont find this output useful, at least I did not understand much from this.
 
