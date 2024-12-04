## Operation BLUE
In this part we learn to use ELK, to analyse logs and investigate a particular attack.
+ Firstly we open Elastic then set the collection to `frostypines-resorts`.
+ After that we set the time frame from 11:30, October 3rd 2024 to 12:00, October 3rd 2024.

### Where was the web shell uploaded to?
Firstly we add `message` and `clientip` columns as those are the info that is of interest to us. Then we add the following filter: `message: "shell.php"` to filter out the logs that include instances of the web shell being uploaded somewhere. Here we can see the path: `/media/images/rooms/shell.php`
### What IP address accessed the web shell?
Firstly we add `message` and `clientip` columns as those are the info that is of interest to us. Then we add the following filter: `message: "shell.php"` to filter out the logs that include instances of the web shell being uploaded somewhere. We get two ip addresses. One does not run any commands thus it is not malicious while the other is running commands thus making it malicious. The ip address which runs these commands is `10.11.83.34`.

## Operation RED
In this part we learn to do the attack we just investigated.
