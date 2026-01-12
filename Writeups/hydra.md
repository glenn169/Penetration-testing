# Hydra 

1. hydra -l <username> -P <path_to_password> http://target_ip/ <service>   [service eg: ftp,ssh,telnet..etc]
2. For bruteforcing the password from website you need to use http-post-form
   hydra -l <username> -P <Path_to_password> http://target_ip/ http-post-form "/<login_end_point>:username=^USER^&password=^PASS^:F=incorrect" -V

