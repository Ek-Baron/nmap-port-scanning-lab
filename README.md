# Project 07 - Nmap Port Scanning and Service Detection Lab
The objective of this lab was to learn how to use Nmap to scan a host for TCP ports and identify services running on open ports.
I performed the experiment in my own controlled Linux environment by first scanning the host, then starting a Python HTTP server on port 8000 and scanning again to observe how the results changed.

## Lab Environment
- device: ASUS Chromebook
- Environment: Linux on ChromeOS
- Tool: Nmap
- Server: Python 3.9.2 SimpleHTTPServer
- protocol tested: tcp
- test port: 8000
- target: local lab host

## Step 1: initial port scan
i first performed a service/version scan against my local lab host.
### Command
nmap -sV <LAB-IP>
### Results
Nmap reported the host was up but all 1000 TCP ports scanned by default were closed
### What this means
The host was reachable on the network but Nmap did not find a service listening on any of the default ports it scanned.

### step 2: starting a local HTTP server
to create a service that Nmap could detect, i started a python HTTP server on TCP port 8000
### command
python3 -m http.server 8000
### Result
python started an HTTP server listening on port 8000. This created an active service on the host that could then be detected during another Nmap scan.

## Step 3: Scanning Port 8000
After starting the HTTP server i performed another Nmap scan, this time specifically targeting TCP port 8000
### command
nmap -sV -p 8000 <LAB-IP>
### Result
Nmap detected TCP port 8000 as open and identified an HTTP service running on it:
8000/tcp open http SimpleHTTPServer 0.6 (Python 3.9.2)
### What This Means
-8000/tcp - TCP port 8000 was scanned.
-open - A service was actively listening on the port.
-http - Nmap identified the service as HTTP.
-SimpleHTTPServer - Nmap identified the HTTP server.
-Pyhton 3.9.2 - Version information associated with the service was detected.

## Step 4: Observing Nmap Service Detection

While Nmap performed service/version detection,i observed activity in the python HTTP server terminal.
The server received several request from Nmap, including HTTP GET and POST request. Some requests returned status codes such as:
- 200 - The request was successful.
- 404 - The requested resource was not found.
- 501 - The HTTP method used by the probe was not supported by the Python Server.

### What i learned
The '-sv' option does more than check whether a port is open. Nmap sends probes to the service and analyzes its responses to help determine what service and version are running on the port. This explains why multiple HTTP requests appeared in the Python server terminal while the Nmap scan was running

## Key Findings
- A host can be reachable even when no scanned ports are open.
- An open port indicates that a service is actively listening for connections
- Starting the Python HTTP server caused TCP port 8000 to become open.
- Nmap's '-sV' option can perform service/version detection on an open port.
- Service detection works by sending probes and analyzing how the service responds.
- Port scanning can help identify services running on a system and provide useful information for troubleshooting and security assessment.
## Skills demonstrated
- Basic TCP port scanning with Nmap
- Service and version detection
- Understanding open and closed TCP ports
- Running a basic HTTP service with Python
- Observing HTTP requests and response status codes
- Basic linux command line usage
- Technical documentation

## Ethical Use
This lab was performed in my own controlled environment for educational purposes.

## Conclusion
This lab helped me understand the relationship between ports and network services. I observed how Nmap reported the host before a service was running, started an HTTP server on port 8000, and then verified that Nmap could detect the newly opened port and identify the HTTP service.
