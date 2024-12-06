On this day we learn to do a `XXE injection`. `XML` or `Extensible Markup Language` is a common language between computers to store and transfer data. At times necessary sanitation is not in place which can allow attackers to extract sensitive info and cause undesirable behaviour. This can be caused by a `XXE injection`.  `XXE` is an attack that takes advantage of how `XM`L parsers handle external entities.

+ Firstly we open up `burp` on the VM and do the necessary adjustments as instructed on TryHackMe's site. Now navigating to `http://MACHINE_IP/product.php` if we add a product to our wishlist we can see the following AJAX call is made to `wishlist.php` with the following XML input.

  ![image](https://github.com/user-attachments/assets/d7b0ca94-124b-4f59-9d1b-9008534d562b)

+ Now we forward this response to the `Repeater` Tab so that we can further inspect it.
+ Now we proceed to checkout and type in our name and address

  ![image](https://github.com/user-attachments/assets/69c941a8-abd5-49af-87ea-e27fd14ac1f1)

+ We get the following response:

  ![image](https://github.com/user-attachments/assets/e6033588-5a34-47ef-9510-f5a6f19527ba)

+ Now going to the `Repeater` tab and modifying our XML input to read `/etc/hosts` we get the following response.

  ![image](https://github.com/user-attachments/assets/e5365be1-0117-4633-9734-e349a457458b)

  ![image](https://github.com/user-attachments/assets/a3025dc9-08d2-48eb-bf29-6065c3da2808)

+ Now we try to read `wish_2.txt` which was initially not accesible by us by modifying the XML input. We get a promising response.

  ![image](https://github.com/user-attachments/assets/e44cbfbf-4585-4f39-b613-ebf6e727de9b)

+ Now we forward this request to the `Intruder` tab and modify our Payload type to `Numbers` and we start an attack that iterates from `wish_1.txt` to `wish_20.txt`, the wish just before our wish.

  ![image](https://github.com/user-attachments/assets/bcb36c26-1324-448b-83e8-4b1edabf3dd8)

+ We get responses for each wish, now we go through each of them to look for our flag and voila we get our flag at `wish_15.txt`

  ![image](https://github.com/user-attachments/assets/3888afaf-ce6e-4295-ab80-e05a52c65c17)

+ Now we look at the `CHANGELOG` to get the flag for the second question. We go to `http://MACHINE_IP/CHANGELOG` to view the changelog.

  ![image](https://github.com/user-attachments/assets/fa06693f-72ff-4dc6-b2e7-de40f8c7efef)

## What I Learned
+ Often times when web applications are rushed through development they overlook how XML data is handled which can lead to XXE vulnerabilities. Thus an attacker can look at sensitive info through an XXE injection.
+ Also learned to use `Burp` to execute a XXE injection.

