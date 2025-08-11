# Web | Robots not allowed

**Name**: Robots not allowed

**Coin**: 75

**Link**: [https://bankctf.vercel.app/](https://bankctf.vercel.app/)

---

## First Step: Application Reconnaissance

I started by navigating through the application and discovered a `/flag` path in the `/robots.txt` file.

![image.png](Web%20Robots%20not%20allowed%2024ca4a579e0d805dad01eb5d5f81e7b6/image.png)

After navigating to the `/flag` path, a file named `file.pcap` was automatically downloaded.

![image.png](Web%20Robots%20not%20allowed%2024ca4a579e0d805dad01eb5d5f81e7b6/image%201.png)

## Network Traffic Analysis

I opened the `file.pcap` file in Wireshark and analyzed the captured network packets. After sorting the packets by length, I found one containing user credentials:

![image.png](Web%20Robots%20not%20allowed%2024ca4a579e0d805dad01eb5d5f81e7b6/image%202.png)

```
E@ec

P09P HTTP/1.1 200 OK
Content-Type: text/html

<html><body>
<!-- Developer Credentials -->
user: john.doe<br>
pass: 2[~MiB55s9Y#<br>
user: jane.smith<br>
pass: 7uHLP|32r!9-<br>
user: developer1<br>
pass: 3,a?9T6PIih<br>
user: developer2<br>
pass: hMC4P8r_!4#}<br>
user: developer3<br>
pass: #5;21B9hIF[n<br>
user: test.user<br>
pass: M!21xdk9KCXk<br>
</body></html>
```

## Credential Testing

After testing all the discovered usernames and passwords, only two accounts worked:

- `developer2` with password `hMC4P8r_!4#}`
- `test.user` with password `M!21xdk9KCXk`

## Hash Discovery and Cracking

Inside the `developer2` account, I found **User Credential Hashes**.

![image.png](Web%20Robots%20not%20allowed%2024ca4a579e0d805dad01eb5d5f81e7b6/image%203.png)

I proceeded to crack these hashes:

![image.png](Web%20Robots%20not%20allowed%2024ca4a579e0d805dad01eb5d5f81e7b6/image%204.png)

Successfully cracked the `admin` password: `ICTAdministrator1991`

## Final Access

After logging in with the admin credentials, I found the flag in the Admin Tools tab by clicking the "Execute Transfer" button.

**Flag**: `flag{B4Nk_h4v3_b3en_pwn3d}`

![image.png](Web%20Robots%20not%20allowed%2024ca4a579e0d805dad01eb5d5f81e7b6/image%205.png)

---

### Key Findings:

- **Initial Discovery**: `/robots.txt` revealed the `/flag` endpoint
- **Network Analysis**: PCAP file contained developer credentials in HTTP traffic
- **Privilege Escalation**: Used developer account to access password hashes
- **Hash Cracking**: Successfully cracked admin credentials
- **Final Exploitation**: Admin panel provided access to the flag