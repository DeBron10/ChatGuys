We can perform a small connectivity test before developing the chat application. This will tell us whether another developer's PC can access a web application running on the host PC.

We won't change network configurations, bypass security controls, or require administrator privileges. We'll test using the connectivity already permitted by your company's LAN.

## Step 1: Check the host PC's IP address

On the developer's PC that will act as the host:

1. Press `Windows + R`.
2. Type `cmd`.
3. Execute:

```
ipconfig
```

Look for the active Ethernet adapter and note:

```
IPv4 Address: 192.168.1.25
```

This is just an example. Note the actual address.

## Step 2: Check whether the host can run a local web application

On the host PC:

1. Open Visual Studio 2012.
2. Create a temporary ASP.NET Web Forms project.
3. Add a simple page containing:

```
<h1>LAN Server Test Successful</h1>
<p>The host web application is running.</p>
```

4\. Run the application using Visual Studio.

It will normally open a local URL similar to:

```
http://localhost:12345/
```

Open it in the host PC's browser.

Expected result

You should see `LAN Server Test Successful`.

## Step 3: Find out whether the application is listening beyond localhost

Open Command Prompt on the host and execute:

```
netstat -ano | findstr :12345
```

Replace `12345` with the port shown in your Visual Studio URL.

You might see:

```
TCP    127.0.0.1:12345    0.0.0.0:0    LISTENING
```

This means the application is listening only on localhost.

If you see:

```
TCP    0.0.0.0:12345     0.0.0.0:0    LISTENING
```

It is listening on all IPv4 interfaces.

This distinction is important: Visual Studio's default development server generally binds to localhost, so a second PC cannot automatically access it.

## Step 4: Test from another developer's PC

On Developer 2's PC, open Command Prompt.

First test basic connectivity:

```
ping 192.168.1.25
```

Replace the IP with the host PC's actual IP.

Then test the application port:

```
Test-NetConnection 192.168.1.25 -Port 12345
```

Look for:

```
TcpTestSucceeded : True
```

If the result is `True`, the TCP connection to that port succeeded.

If it is `False`, the port is not reachable. Possible reasons include the server binding, firewall rules or network restrictions.

Finally, try opening the host URL in Developer 2's browser.

## Step 5: Understand the possible results

| Test result              | Meaning                             |
| ------------------------ | ----------------------------------- |
| Host opens localhost URL | Local web server works              |
| Ping succeeds            | Basic network connectivity exists   |
| Ping fails               | ICMP may be blocked; not conclusive |
| TCP test succeeds        | Host port is reachable              |
| TCP test fails           | Port isn't reachable from that PC   |
| Browser opens page       | LAN web access works                |

## Let's do this in a controlled way

I suggest we first perform Steps 1 and 2.

Then share these three results with me:

Host PC connectivity details

1\. Host PC IPv4 address

2\. Visual Studio application URL

3\. Have you tested from another PC?

Not tested yet

Yes, browser opened successfully

No, connection failed

Submit test results

Important: Don't change IIS bindings, firewall settings, network adapters or security policies just to make the test pass. If access is blocked, we'll identify exactly where and determine what can be done within the permissions already available.

Our first milestone is simple: get one test page running on the host and open it from a second developer's browser. Once that works, we can proceed with SignalR.
