# MaxHashMining Google Script to send an email if your rig's hashrate is below a set value

# Instructions
1) Copy the contents of  MaxHashEmailNotification.txt  into google sheets script editor. 
2) Change the wallet address along with your email in the script.
3) In the script editor, click on the clock icon in the menu to set the project triggers.
4) Set the project triggers to update every 5 min for myFunction and sendEmail function (Maxhash limit is 1 request evey 5s per IP)
5) Save and accept google permissions
6) Go to cell B2 and set it to "=myFunction(A1)"  
7) Sit back and relax, you will now get updates if youur hasrate falls below the limit that you set

Tips ETH : 0xa3da841731c79097126796488922c22951d8fd53

