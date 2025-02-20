### Lab 2.1 - SEC-350

#### Today's mission is to synchronize all of our timezones across our boxes. This is important so that in our Syslog entries, we can see when events are actually recorded in a consistent timeline.

The config files are all in /etc/rsyslog.conf by the way.

So, we are gonna edit our rsyslog configuration file and comment out the highlighted line on rw01:

![image](https://github.com/user-attachments/assets/9e590a51-bfc4-47ab-b107-f52ef4aa2ef5)

Let's do the same for web01:

![image](https://github.com/user-attachments/assets/af054c51-719e-4b87-8ca8-e81990d56d74)

And log01:

![image](https://github.com/user-attachments/assets/859ed940-22a4-4ea8-a217-edbb3f7dad64)

Remember to restart rsyslog afterwards.

The before and after should look like this!

![image](https://github.com/user-attachments/assets/8aeb0e6a-0c70-4f5c-bc29-a8d826f430f1)
