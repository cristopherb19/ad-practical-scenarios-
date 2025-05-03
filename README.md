<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory: Practical Scenarios</h1>
Welcome back again!  This project is all about simulating scenarios that may be encountered when working with Active Directory.  By the end of this project, you should have a better understanding of user provisioning and administration within Active Directory.
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Prerequisites</h2>

-  Please complete the following before beginning this project:
    - <a href="https://github.com/cristopherb19/active-directory-setup">Active Directory: Setup with Azure Virtual Machines</a>
    - <a href="https://github.com/cristopherb19/ad-deploy-and-config">Active Directory: Deployment and Configuration</a>
    - <a href="https://github.com/cristopherb19/ad-user-generation">Active Directory: Generating Users</a>

<h2>Objectives</h2>

- Learn more about the capabilities of domain administrations within Active Directory
- Learn more about the challenges users connected to the domain may encounter and how they are resolved

<h2>Scenarios</h2>

<h3>&#9312; Configuring Account Lockout Policy</h3>
<h4> Situation: </h4>
<p>
    
Many user accounts are being accessed by unauthorized users.  Because of this we want to create an account lockout policy for all users and computers in the domain to prevent unauthorized users from accessing these accounts.  This policy will define settings that control when an account is locked after multiple failed login attempts, how long the account stays locked, and how the lockout counter is reset.
  
</p>
<br />
<h4> Steps: </h4>

<p>

- Log in to DC-1 VM via Remote Desktop
- Within DC-1 VM, open "Group Policy Management" application
- Within "Group Policy Management" application, right click and "Edit" the "Default Domain Policy"
<img src="https://imgur.com/1SDywvc.png alt="default domain policy"/>

- Expand "Computer Configuration" -> "Policies" -> "Windows Settings" -> "Security Settings" -> "Account Policies" -> "Account Lockout Policy"
- Open "Account lockout duration" (NOTE: This is how long user accounts will be locked)
- Check "Define this policy setting" box, designate a time, and then click "Apply"
<img src="https://imgur.com/bNbAFFQ.png" alt="account lockout duration"/>

- Click "Account lockout threshold" (NOTE: This is how many login attempts a user can fail before account gets locked)
- Check "Define this policy setting" box, designate number of invalid login attempts, and then click "Apply"
<img src="https://imgur.com/yCJNZAg.png" alt="account lockout threshold"/>

- Confirm that "Allow Administrator account lockout" is "Enabled"
- Click "Reset account lockout counter after" (NOTE: This is how long account is locked for before allowing user to attempt login again)
- Check "Define this policy setting" box, designate a time, and then click "Apply"
- Review policies and policy settings
<img src="https://imgur.com/ldXbKsP.png" alt="policies"/>
<img src="https://imgur.com/WDVvp0E.png" alt="policies"/>
    
</p>
<br />
<h4> Result: </h4>
<p>
    
As the domain administrator, we have successfully created an Account Lockout Policy to protect the accounts of users in our domain from being accessed by unauthorized users.  User accounts will now be locked and inaccessible after too many unsuccessful login attempts.
    
</p>
<br />

<h3>&#9313; Dealing with Locked User Accounts</h3>

<h4>Situation:</h4>
<p>

A user is unable to log in to their device because their account has been locked for security purposes.  The user seeks help from the domain administrator, since they have full control over the domain, which includes the ability to unlock their account. 

- NOTE: To simulate this we will first have to choose a user from our active directory, then purposely fail to login to Client-1 VM with that selected user's username until the account gets locked. (EXAMPLE: If user's name is "fox.civ", then the username you should use when attempting to login is "mydomain.com\fox.civ")
    
</p>
<br />
<h4>Steps:</h4>
<p>

- Within DC-1 VM, open "Active Directory Users and Computers" application
- Within "Active Directory Users and Computers" application, right click "mydomain.com" then click "Find"
- Under "Name", type in the name of the user with the locked account to find them
<img src="https://i.imgur.com/WIvwCtT.png" alt="find user"/>

- Double click the found user to open their properties
- Navigate to "Account" tab
- Check box that says "Unlock account. This account is currently locked out on the Active Directory Domain Controller"
<img src="https://i.imgur.com/Lib2ASJ.png" alt="unlock user account"/>

- Click "Apply"
    
</p>
<br />
<h4> Result: </h4>
<p>

As the domain administrator, we assisted the user by unlocking their account.  The user has regained full access and is now able to login successfully.

- NOTE: You could test to see if it worked by simply logging in to Client-1 VM with the user's correct login credentials.
    
</p>

<br />
<h3>&#9314; Account Activation and Deactivation</h3>

<h4> Situation: </h4>

<p>

A user has reason to believe their account information has been leaked and there is a chance an unauthorized user may have gained access to it.  The user reports this to a domain administrator so they can take action in ensuring the security of company resources.  The domain administrator will now want to temporarily deactivate the account and check the logs for any suspicious activity.
    
</p>
<br/>

<h4> Steps: </h4>

<p>

- Log in to the DC-1 VM as "Jane Doe" (Domain Admin)
- Within "Active Directory Users and Computers" application, right click "mydomain.com" then click "Find"
- Under "Name", type in the name of the user with the locked account to find them
<img src="https://i.imgur.com/WIvwCtT.png" alt="find user"/>

- Right click the found user and click "Disable Account"
<img src="https://i.imgur.com/feJNNrv.png" alt="disabling account"/>

- Now that the account is disabled, we will check the logs for suspicious activity
- Log in to Client-1 VM as "Jane Doe"
- Within Client-1 VM, type in "eventvwr.msc" in the windows search bar and open the application
- Navigate to "Windows Logs" -> "Security"
- Observe the logs and you will see that there have been some "Audit Failures"
- If you look at the details, you will see that these particular "Audit Failures" are attempted login attempts that failed, which indicates that someone has been continuously trying to access the account
<img src="https://i.imgur.com/JNe6SJd.png" alt="disabling account"/>

- At this point the domain admin could contact the user to verify if these failed login attempts was them
- If the user says that they were not the cause of the failed login attempts, then the domain administrator will reset their password to further protect their account.  To do this, go back to the Active Directory where we found the user, right click the user, and select "Reset password" to reset their password
<img src="https://i.imgur.com/bytZdWr.png" alt="reset password"/>

- Now that the domain administrator has reset their password, they can reactivate their account.  To do this, you will just right click the user and click "Enable Account".  (This is the same way you previously disabled account and reset password)
<img src="https://i.imgur.com/bytZdWr.png" alt="enable account"/>
    
</p>
<br />

<h4> Result: </h4>
<p>

As the domain administrator, we have successfully reviewed the user's account activity and protected company resources.  The user's account was deactivated, observed for suspicous activity, and re-enabled with new login credentials.  The user is now relieved that their account information is protected again.
  
</p>
<br />

<h2>Conclusion</h2>

<p>

Congratulations on completing this series of Active Directory projects!  Active Directory is absolutely crucial for managing network access and security within an organization.  These simulated situations are common challenges an IT professional may encounter in the workplace and it is important to have an idea of how to resolve them.

</p>
