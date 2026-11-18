# Security

## Trust Boundaries
* Sensor and Server
* Server and Database
* Host Client and Server
* Guest Client and Server
## Authentication and Authorization
Host accounts will use a Google Account to log-in.

Guests will not have an account and everything they have access to on the client will be read-only.

Sensors will have a hardware-protected private key stored and the server's public key stored in it.
## Secrets and Data Protection
The Raspberry Pi will have a digital signature check when booting up to see if the code has been tampered with, it won't run the code.  
## Threats and Mitigations
| Threat | Risks | Control |
|---------|---------|------|
| Someone spoofs as a sensor and tries to send false vehicle events to the server. | Accepting false vehicle events can lead to inaccurate data in pools, making our counting service less reliable for hosts.   This may also enable to malicious hackers to send worse things to the server.| Authenticate sensors with a unique security key.   This private key will accociated with the serial number printed on the sensor and is only known to the admin and is validated server-side.  Make sure that the only type of information the server accepts past the trust barrier between server and sensor are vehicle events in a certain format.|
| Old vehicle events get resent to server. | Causes pool counts to become inaccurate and unreliable. | Give vehicle events unique IDs and reject the data if the timestamp and ID overlaps. |
| Someone tries to brute force guess a host's credentials. | Host's pool information will be exposed if successful, allowing them and the sensors to be viewed, edited, and deleted. | Rate limit unsucceful logins from an IP, 5 failed attempts will block login attempts from the IP for 24 hours. |
| DDOS on a server by spamming it with attempts at giving vehicle event data. | Disrupts the server's ability to handle real vehicle event data incoming. | The server will be the one requesting updates from the sensors periodically and only expect and accept responses after sending these requests.   If vehicle event data is sent to the server by something other than a sensor it is expecting a response from, the data will be ignored. |
| A sensor gets physically stolen or tampered with. | Row5C2 | Row5C3 |