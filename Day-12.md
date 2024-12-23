Today we asked to perform a `Timing Attack` specifically a `Race Condition Attack`.

If we look at the source code provided to us:
```
 if user['balance'] >= amount:
        conn.execute('UPDATE users SET balance = balance + ? WHERE account_number = ?', 
                     (amount, target_account_number))
        conn.commit()

        conn.execute('UPDATE users SET balance = balance - ? WHERE account_number = ?', 
                     (amount, session['user']))
        conn.commit()
```
We can see that a `SQL` command is being executed and then the database gets updated with `.commit()`. `SQL` commands usually take more time to execute than CPU operations such as the balance check we are performing. This develops a possibility of a race condition. If we are to send multiple requests then we would pass the balance check multipile times simultaneously before we reach `.commit()` and the database gets updated. Our task is to exploit this vulnerability.

Firstly we enter the credentials given to us to enter into the bank's web application. Now we open a burpsuite and send a initial request to acc 111 and 1000 dollars. Now we copy that request and make a new tab group and copy and paste the same request multiple times. 

Now we if we refresh the webpage we see that our balance is in negative and we are given the flag.
