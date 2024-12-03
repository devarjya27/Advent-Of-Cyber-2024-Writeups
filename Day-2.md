On this day we learn to use Elastic SIEM (System Information and Event Management) to look at various logs and classify alerts into True Positives and False Positives.
+ Firstly we login to Elastic, navigate to the Discover tab then set the required timeframe.
+ We then add columns of the logs that are of interest to us.
+ We then apply the filters as and when required.

### What is the name of the account causing all the failed login attempts?
By adding the `user.name` column and filtering the `event.outcome` to `failure` we see that the username of the account that cause all the failed login attempts is `service_admin`.

### How many failed logon attempts were observed?
By filtering the `event.outcome` to `failure` we can see that there were `6791` hits.

### What is the IP address of Glitch?
By adding the `source.ip` column we can correlate the ip `10.0.255.1` to the `host.hostname` `ADM-01`.

### When did Glitch successfully logon to ADM-01? Format: MMM D, YYYY HH:MM:SS.SSS
By filtering for `host.hostname` `ADM-01` we get the answer.

### What is the decoded command executed by Glitch to fix the systems of Wareville?
By decoding the encoded text from base64 to UTF-16LE we get the the powershell script `Install-WindowsUpdate -AcceptAll -AutoReboot`.

## What I Learned
Analysts use a SIEM system to classify logs or alerts to True Positives and False Positives and take further action if it is a True Positive. False Positives arise when admins from within the organization debug something. True Positives arise when an attacker has malicious intentions and attacks the organization's systems.
