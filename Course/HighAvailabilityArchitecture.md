
ELBs:
Application (Lvl 7 HTTP/S) - for intelligent Routing

Network (Lvl 4 TCP ) - for fastest performance 

Classic ELB - cheapest one, wihtout intelligence
  If you need IPv4 of user in Classic ELB - look for X-Forwarded-For header 

504 Error (Gateway timed out) - application not responding within Idle Timeout period

Instances monitored by ELB either status: InService or OutService.
Instance states are base on Health Checks .
Load Balancers have DNS name, you never given IP address


Do read ELB AWS FAQ  (10 questions in exam on ELB)



