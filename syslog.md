### Lab 1 - SEC-350 - Syslog Entry

#### Here is the assignment:

Syslog Tech Journal Entry. Take notes on the configuration steps necessary to create a syslog server and a syslog client.

#### So, in my Lab 1.1, I covered how to set up our Web01 and Log01 boxes, which are rsyslog client and rsyslog server respectively. But let's talk about what Syslog is in the first place.

The term syslog refers to a standard method for message language and became part of the standard message logging solution for Unix-based operating systems. The term syslog simultaneously refers to:
1. A generic term for system logging
2. The name of the original tool implementation tool (more specificially managed by a system daemon called syslogd)
3. The name of the client/server protocol (RFC3164/RFC5424) that allows for message logging across multiple hosts.
4. The name of the physical logging format of the file content for the system log

There many implementations of the original syslog service, all are often referred to as a syslog, even though the actual command tool supporting that functionality may have a different name. What we are using for class is called rsyslog, though.

#### So syslog is just system logging from what we want to monitor to a centralized location. Thanks, [LogScale](https://library.humio.com/kb/kb-syslog-rsylog.html). 

#### Now, if you want to see how I set it up, you can check it out in my [Lab1.1](https://github.com/seraphimgerber/SEC350/blob/main/Lab1.1.md#now-we-will-set-up-log01-heres-a-checklist-to-follow). 
