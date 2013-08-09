[*Link: Detail Manual - Korean*](http://www.ralfyang.net/Foswiki/bin/view.cgi/Main/TheZinst) 한국어 지원
* [*Link: zinst starters package set in github*](https://github.com/goody80/Ralf_Dev)
* Contact: goody80762@gmail.com or ralf.yang@gsshop.com

# Zinst
## What is the Zinst ?
### zinst?
* Package install manager. It very similar that concept of yinst command in Yahoo!

### Summary
* For the centralized package manage & distributed systems
  * Centralized control:
    * Install the Package to the destination server 
      * *ex) zinst install apache_server-1.0.1.zinst apache_conf-1.0.1.zinst -h web0[1-9]* 
    * list-up the package in each server 
      * *ex) zinst ls*
    * list-up the file of package in each server 
      * *ex) zinst ls -files apache_server*
    * Easy find out the installed package-name of a some distributed file 
      * *ex) zinst ls -files /data/z/httpd/conf/include/_temp.conf*
    * Can tracking the release history with who could controlled
      * *ex) zinst history*
    * Easy can change the configuration setup 
      * *ex) zinst set apache_conf.maxclient=64*
      * Then you can see the configuration has been changed on the Apache server for example.
    * Package remove
    * Send a command to the distributed systems
      * *ex) zinst ssh "whoami" -h web[0-1][0-9], web20*
    * Can makes a list of multiple host for the target control
      * *ex) zinst ssh "whoami" -H ./hostlist.txt* 
    * One package, can makes a differnt output
      * *ex) zinst install apache_server -set apache_server.maxclient=32 -h web01 web02*
      *     *zinst install apache_server -set apache_server.maxclient=64 -h news01 news02*
    * Daemon controll
      * *ex) zinst start httpd*
      * Then we can recognize that who managed the daemon in the server as a history
    * Supported a package restore & roll-back as a save file - Not completed yet
      * *ex) zinst restore -file /data/z/save/zinst-save.56*
    * Without difficult language and environment. Due to it made by Bash only

### How to install
```
sh ./install.sh

 ======================================================================================== 
  Please insert directory name for you want (ex /data )
  Directory will be created as you type it 
 ======================================================================================== 
/data
 ======================================================================================== 
  Do you want to make a Distribute server for packages download? [y / n ]
  * Notice: Apache web server will be start in this server automaticlly *
 ======================================================================================== 
y
 ======================================================================================= 
  File directory for the Distritution is /data/dist  
 ======================================================================================= 
 ======================================================================================== 
  Please insert your Domain name of the Distribution server ex)package.dist.blahblah.com
 ======================================================================================== 
package.dist.blahblah.com
 ======================================================================================= 
  Please insert your IP address of the Distribution server
 ======================================================================================= 
192.168.10.141
 =======================
 | Go ahead ? [y or n] |
 =======================
```
* And please rejoin the server for the path apply

```
$] zinst -help
```


### How to use
<pre>
zinst help
------------------------------------------------------------------------------------------------------ 
	zinst	[Command]	[Option Types]		[Target Names]	[-h or -H]	[Targe Host] 
------------------------------------------------------------------------------------------------------ 
 + For remote work 
 
  - Remote control: You can send a command to seperated hosts 
		 ssh		[Command]						*Host requires	  
...................................................................................................... 
 
  - File copy to remote: You can send a file(s) to seperated hosts(mcp = Multi CoPier) 
		 mcp		[local-files]		[Destination DIR]		*Host requires 
 
------------------------------------------------------------------------------------------------------ 
 + For Package 
 
  - Package manage: You can install/remove a package as under the command 
		 install				[Package]   
				[-same]			[Package]	  
				[-downgrade]		[Package]	  
				[-stable]		[Package without version for latest package]	  
		 remove					[Package]	  
...................................................................................................... 
 
  - Package view: You can see an installed packages/files/index & dependency 
		 list					[Blank for list-up]	  
				[-files]		[Package]	  
				[-files]		[/Dir/File-name]	  
				[-zicf]			[Package]	  
				[-dep]			[Package]	  
...................................................................................................... 
 
  - Package restore: You can restore the package set as a file for restore	ex) ~/z/save/zinst-* 
		 restore	[-file]			[Saved file_name]	  
				[-igor]			* Not available yet 
 
------------------------------------------------------------------------------------------------------ 
 + For Configuration 
 
  - Configuration: Zinst can helps to configure the setup without manual modify the Conf-file 
		 set					[Blank for list-up]			  
							[Package.option=value]			  
 
  - Configuration with Install: Configure the setup with the package install 
		 [Package]	-set 			[Package.option=value]  
 
------------------------------------------------------------------------------------------------------ 
 + For System manage 
 
  - Daemon control: You can control the daemon from the /etc/init.d/ directory 
		 start					[Daemon_name]	  
		 stop					[Daemon_name]	  
		 restart				[Daemon_name]	  
...................................................................................................... 
 
  - Crontab manage: You can touch the cron schduler by zinst 
		 crontab	[-e]						  
				[-l] 
 
------------------------------------------------------------------------------------------------------ 
 + For install available package find 
 
  - Package find 
		 find		[Blank for list-up]			 
				[Package]		 
 
------------------------------------------------------------------------------------------------------ 
 + View history 
 
		 history	[Number of Range] 
...................................................................................................... 
 
		 self-update			 
		 -version			 
 
		 *, help		 
------------------------------------------------------------------------------------------------------ 
 -h is target host, -H is targe file of hostlist 
 ex) zinst i sample-1.0.0.zinst -h web01.news.kr[1,3]  web[03-12].news[1,3] 
 ex) zinst i sample-1.0.0.zinst -H ./server_list.txt 
------------------------------------------------------------------------------------------------------ 
 
 
 
Example)
zinst ssh 'cat /etc/hosts;pwd' -h web[01-09].test.com	-- Send a command to seperated hosts 
zinst mcp ./test.* /data/var/ -h web[01-09].test.com 	-- File copy to seperated hosts 
 
zinst install hwconfig-1.0.2.zinst -same		-- for overwrite the package as a same version 
zinst i hwconfig-1.0.2.zinst -downgrade			-- for downgrade the package as a lower version 

zinst install hwconfig -stable			-- for package apply as a latest version automatically 

zinst list -files hwconfig				-- list-up file of the hwconfig package 
zinst ls -files /data/bin/hwconfig			-- find a package as a file 
zinst list -zicf hwconfig				-- see the index file of package 
zinst ls -dep hwconfig 					-- package dependency check 
 
zinst set						-- list-up of zinst current setups 
zinst set hwconfig.nameserver1=1.1.1.1			-- change the setup nameserver1=1.1.1.1 to the hwconfig 
 
zinst i hwconfig-1.0.2.zinst -set hwconfig.nameserver1=1.1.1.1 -set hwconfig.nameserver2=2.2.2.2 
 -- change the setup nameserver1=1.1.1.1 and nameserver2=2.2.2.2 to the hwconfig with package install 
 
zinst restart httpd					-- restart the httpd daemon by /etc/init.d/httpd file control 
 
zinst crontab -l 					-- list-up the crontab scheduler 
zinst crontab -u root -l			-- list-up the crontab scheduler for an user 
zinst cront -e	 					-- edit the crontab scheduler 
 
zinst find						-- list-up the available file for install 
zinst find hwcon					-- list-up the available file for install as you typed 
 
zinst hist						-- show the history 
zinst hist 300						-- show the 300 lines history 
 
zinst self-update					-- zinst command update( *Requires: Package dist server must has a zinst file) 
 
zinst help						-- Detail view the help 
 
=== For more detail: http://www.ralfyang.net/Foswiki/bin/view.cgi/Main/TheZinst ===
</pre>

