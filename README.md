# ChatGuys

Yes. Tomorrow, just perform these checks in order. We don't need to build the chat application yet.

### Tomorrow's test checklist

1\. On the host PC

- Open Command Prompt.
- Run `hostname` and note the PC name.
- Run `ipconfig` and note the IPv4 address.
- Open your test ASP.NET Web Forms project in Visual Studio 2012.
- Run it and note the complete local URL, including the port number.

2\. Check whether the application is listening

Open another Command Prompt and run:

```
netstat -ano | findstr :PORT_NUMBER
```

Replace `PORT_NUMBER` with the port from your local URL.

3\. From another developer's PC

- Try opening the host's URL using its IPv4 address instead of `localhost`.
- If it doesn't open, run this in PowerShell:

```
Test-NetConnection HOST_IP_ADDRESS -Port PORT_NUMBER
```

Replace both values with the actual host IP and port.

4\. Check hostname resolution

From the second PC, run:

```
ping HOST_PC_NAME
```

Note whether the hostname resolves to the host's IP address. Ping itself may fail even when hostname resolution works.

### Send me these results tomorrow

| Information                         | Result |
| ----------------------------------- | ------ |
| Host PC name (`hostname`)           |        |
| Host IPv4 address                   |        |
| Visual Studio local URL             |        |
| `netstat` output                    |        |
| Can the second PC open the website? |        |
| `TcpTestSucceeded`                  |        |
| Does the host PC name resolve?      |        |

You can simply paste the command outputs here; screenshots are also fine.

One important thing: Don't change firewall rules, IIS settings, network settings, or request elevated permissions just to make the test pass. We first want to understand what already works under your company's existing configuration.

Once we have these results, we'll know whether the temporary-host approach is technically feasible before writing the actual chat application.
