
## Cisco Routers Password Types:  
-------------------------------------------  
### Type 0  
This mean the password will not be encrypted when router store it in Run/Start Files  
Command:  
```
enable password cisco123
```

### Type 4
This mean the password will be encrypted when router store it in Run/Start Files using SHA-256  
which apps like Cain can crack but will take long time  
Command:  
```
enable secret 4 Rv4kArhts7yA2xd8BD2YTVbts
```
(notice above is not the password string it self but the hash of the password)

This type is deprecated starting from IOS 15.3(3)

### Type 5
This mean the password will be encrypted when router store it in Run/Start Files using MD5  
which apps like Cain can crack but will take long time  
command:  
```
enable secret 5 00271A5307542A02D22842
```
(notice above is not the password string it self but the hash of the password)  
or 
```
enable secret cisco123**
```
(notice above is the password string it self)

### Type 7
This mean the password will be encrypted when router store it in Run/Start Files using Vigenere cipher  
which any website with type7 reverser can crack it in less than one second  
command :  
```
enable password cisco123 
service password-encryption
```

### Type 8
This mean the password will be encrypted when router store it in Run/Start Files using PBKDF2-SHA-256
starting from IOS 15.3(3).
Password-Based Key Derivation Function 2 (PBKDF2) with Secure Hash Algorithm, 26-bits (SHA-256) as the hashing algorithm

Example:
```
R1(config)#**enable algorithm-type sha256 secret cisco**
R1(config)#do sh run | i enable
enable secret 8 $8$mTj4RZG8N9ZDOk$elY/asfm8kD3iDmkBe3hD2r4xcA/0oWS5V3os.O91u.
```

Example:
```
R1(config)# **username yasser algorithm-type sha256 secret cisco**
R1# show running-config | inc username
username yasser secret 8 $8$dsYGNam3K1SIJO$7nv/35M/qr6t.dVc7UY9zrJDWRVqncHub1PE9UlMQFs
```

### Type 9
This mean the password will be encrypted when router store it in Run/Start Files using scrypt as the hashing algorithm.
starting from IOS 15.3(3)

Example:
```
R1(config)#**ena algorithm-type scrypt secret cisco**
R1(config)#do sh run | i enable
enable secret 9 $9$WnArItcQHW/uuE$x5WTLbu7PbzGDuv0fSwGKS/KURsy5a3WCQckmJp0MbE
```

Example:
```
R1(config)# **username demo9 algorithm-type scrypt secret cisco**
R1# show running-config | inc username
username demo9 secret 9 $9$nhEmQVczB7dqsO$X.HsgL6x1il0RxkOSSvyQYwucySCt7qFm4v7pqCxkKM
```