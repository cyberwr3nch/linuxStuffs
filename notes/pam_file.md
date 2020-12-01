### PAM File:

***
Pam Configuration file, Pluggable Authentication Module<br />
**_Location:_** */etc/pam.conf*<br />
**_Use:_** Responsible for authentication mechanism of system running applications, Can be edited to change hte authentication mechanism of applications
***

- Pam Configuration file is responsible for authentication of softwares
- Stacking feature is provided to let you `authenticated` users through multiple services
- PAM file also provides password mapping feature, which doesnt require the users provide their passwords multiple times
- It instructs the application on what type of authenticaation mechanism to be involved
- It provides the users the access to the files if the authentication is successful
- The authentication for an application can be changed by making changes in the PAM file
- PAM can restrict certain programs to be authenticated, allowing only certain users to authenticate and warn if a program tries to authenticate all based on the PAM config file
- PAM is installed in most of the latest debian based distributions

#### PAM Modules:
- Every PAM modules implements specific authentication mechanism
- When implementing PAM authentication, PAM module name and module type needs to be specified

#### PAM Module types:
- Specifies what type of authentication to be used by this module

| Module name | Description | Example |
| ----------- | ----------- | ------- |
| auth |  set, refreshes or destroys the creadentials, responsible for acocunt identification | sudo -l {proving password(authentication) for identification}
| account | Determines if the user is allowed to access the service, weather their password is expired etc.., | Docker group members allowed to use docker service
| session | Providing the authenticated users, the permissions and datas to do stuff. Logging is also an prior function of this module  | only an authenticated smb user can access the shares 
| password | Provides user mechanism for chaning their authentication | Changing the password of the users

#### PAM Control Flags:
- The control flags in PAM module determines the progress after authentication, either on successful authentication or failed authentication

| Flag name | On Success | On Failure |
| --------- | ---------- | ---------- |
| required  | records a required success and continue checking for other modules | Calls all other modules listed for this service before denial of authentication |
| requisite | records a required(another control flag) success and continue checking for other modules  | Immediate denial of authentication |
| optional  | record an optional success and continue checking the modules on the stack | record an optional failure and continue checking the stack |
| sufficient | records a success if no preceeding flags with `required` value is failed. On success, it skips checking the remaining modules| records an optional(another control flag) error and continues checking the modules in the stack|

#### PAM Module Path:
- Tells PAM to use the following module to be used for authentication
- Mostly the name for the PAM module is specified in the file, by defalut the pam_* files are located in the `/usr/lib/security/` (/usr/lib/x86Architecture/security/pam_unix.so in Linux2020 VM)
- locate `pam_unix.so` specifies the location of the pam files

#### PAM Module Arguments:
- The arguments to be passed for the module

#### PAM Configuration Files:
- PAM configuration files are located in `/etc/pam.d`
- The PAM configuration files are concatinated and can be found together in `/etc/pam.conf` file in some distros
- Each PAM config files have the following syntax:
  ```bash
  type control module-path module-arguments
  ```


#### PAM file example:
```bash
# Module-type  control-Flag  Module-Name Module-Argument
session    optional   pam_motd.so noupdate                                                                                                             
session [success=ok ignore=ignore module_unknown=ignore default=bad] pam_selinux.so open                                                               
session       required   pam_env.so readenv=1                                                                                                          
session       required   pam_env.so readenv=1 envfile=/etc/default/locale                                                                              
@include common-auth                                                                                                                                   
auth       optional   pam_group.so                                                                                                                     
session    required   pam_limits.so                                                                                                                    
session    optional   pam_lastlog.so                                                                                                                   
session    optional   pam_mail.so standard                                                                                                             
session    optional   pam_keyinit.so force revoke                                                                                                      
@include common-account                                                                                                                                
@include common-session                                                                                                                                
@include common-password    
```


##### Resources:
[Kernal.org](https://mirrors.edge.kernel.org/pub/linux/libs/pam/)<br />
[Redhat.com](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/managing_smart_cards/pluggable_authentication_modules#About_PAM)<br />
[tldp](https://tldp.org/HOWTO/User-Authentication-HOWTO/x115.html#:~:text=When%20this%20is%20the%20case,found%20in%20%2Flib%2Fsecurity.)<br />
[oracle](https://docs.oracle.com/cd/E19683-01/816-4883/pam-32/index.html)<br />
