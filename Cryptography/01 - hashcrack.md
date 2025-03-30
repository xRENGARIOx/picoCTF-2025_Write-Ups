# hashcrack

## Description
A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server? Access the server using `nc verbal-sleep.picoctf.net 57356`

## Solve
1. Let's use netcat to connect to the server:
```sh
nc verbal-sleep.picoctf.net 57356
```

2. The server says the following:
```
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash:
```
Since it is an identified hash, let's use crack station to see if it is in the database:
```
hash:482c811da5d5b4bc6d497ffa98491e38
type:md5
result:password123
```

3. Let's put the result to see what's next.
```
Enter the password for identified hash: password123
Correct! You've cracked the MD5 hash with no secret found!

Flag is yet to be revealed!! Crack this hash: b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
Enter the password for the identified hash: 
```
We got another hash, so let's repeat the process:
```
hash:b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
type:sha1
result:letmein
```

4. Let's see if there is more to do:
```
Enter the password for the identified hash: letmein
Correct! You've cracked the SHA-1 hash with no secret found!

Almost there!! Crack this hash: 916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
Enter the password for the identified hash: 
```
Another one, so let's repeat the process one more time:
```
hash:916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
type:sha256
result:qwerty098
```

5. Finally we got the flag:
```
Enter the password for the identified hash: qwerty098
Correct! You've cracked the SHA-256 hash with a secret found. 
The flag is: picoCTF{UseStr0nG_h@shEs_&PaSswDs!_dcd6135e}
```

```flag
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_dcd6135e}
```

## Additional Notes 

## References
https://crackstation.net