# 4.Execution_of_NetworkCommands
## AIM :
To implement a client-server program using the Ping command to calculate and display the minimum, average and maximum response times in milliseconds.
## Software : Command Prompt And Network Protocol Analyzer

## Procedure: To do this EXPERIMENT- follows these steps:
1. Start the program.
2. Import the required modules: socket, subprocess and re.
3. Create a socket and bind it to localhost at port 8000.
4. Make the server listen for client connections.
5. Accept the client connection.
6. Receive the website hostname from the client.
7. Execute the ping command with 4 requests using subprocess.
8. Capture the ping command output.
9. Extract the minimum, maximum and average response times using regular expressions.
10. If ping statistics are available, display the minimum, average and maximum response times in milliseconds.
11. Otherwise, display "Host Not Found or Ping Failed".
12. Send the results to the client.
13. Repeat the process until the client enters `exit`.
14. Close the client and server sockets.
15. Stop the program.
## Program:
server:
```
import socket
import subprocess
import re

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)

print("Server is waiting for connection...")

c, addr = s.accept()
print("Connected to:", addr)

while True:
    hostname = c.recv(1024).decode()

    if not hostname:
        break

    if hostname.lower() == "exit":
        break

    result = subprocess.run(
        ["ping", "-n", "4", hostname],
        capture_output=True,
        text=True
    )

    match = re.search(
        r"Minimum\s*=\s*(\d+)ms.*Maximum\s*=\s*(\d+)ms.*Average\s*=\s*(\d+)ms",
        result.stdout
    )

    if match:
        min_time, max_time, avg_time = match.groups()
        response = (
            f"Minimum: {min_time} ms\n"
            f"Average: {avg_time} ms\n"
            f"Maximum: {max_time} ms"
        )
    else:
        response = "Host Not Found or Ping Failed"

    c.send(response.encode())

c.close()
s.close()
```
client:
```
import socket

s = socket.socket()
s.connect(('localhost', 8000))

while True:
    ip = input("Enter the website you want to ping: ")

    if ip.lower() == 'exit':
        break

    s.send(ip.encode())

    result = s.recv(4096).decode()
    print("\nPing Statistics:")
    print(result)

s.close()
```
## Output
<img width="1098" height="318" alt="image" src="https://github.com/user-attachments/assets/f064aa4f-554c-407f-8ab6-01062e84ffb6" />


## Result
Thus Execution of Network commands Performed 
