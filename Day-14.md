Today we learn about `self-signed certificates`, `man-in-the-middle attacks` and using burpsuite to intercept traffic. A certificate is basically a pair of cryptogrpahic keys - apublic and a provate key. The public key is made available to the public with which they can encrypt the data they send to the server which can be decrypted by the private key which is held by the server or the website.

To get the certificate details we navigate to `gift-scheduler.thm` and we can see that the name of the `CA` is `THM`.

Open Burp Suite and Once Burp Suite loads, we will select Proxy (number 1 in the screenshot above) and then toggle off the Intercept on option (number 2) to prevent users from noticing any delays in the website responses. Finally, let's open the Proxy Settings (number 3) to set a new listener on our AttackBox IP address.

Now, Turn of the Proxy.

Open a terminal and navigate to /Rooms/AoC2024/Day14 using the following command cd /Rooms/AoC2024/Day14

Now run the script using ./route-elf-traffic.sh
In Burp, Navigate to the HTTP history tab Under Proxy and inspect the requests. and we get the password i.e. `c4rrotn0s3`.

Logging into the account we get the flag `THM{AoC-3lf0nth3Sh3lf}`

Now trying to log in to `Marta Mayware`'s account we look into the requests and get the password which is `H0llyJ0llySOCMAS!`

Logging into the account we get the flag `THM{AoC-h0wt0ru1nG1ftD4y}`
