### Updating the printer



1\. Back up all your cfg files somewhere just in case

![alt text](images/updating/backup-files-1.png?raw=true)
![alt text](images/updating/backup-files-2.png?raw=true)

2\. recover any that require recovery

Note: fluidd-config might not want to recover - This should fix itself later

![alt text](images/updating/overview-out-of-date.png?raw=true)

3\. Reboot host - This is to make sure the repos and installs are all in a safe state to update

![alt text](images/updating/5-reboot-host.png?raw=true)

4\. Update OS Packages - this will take SEVERAL minutes

5\. Update Moonraker - also takes several minutes, Moonraker will then restart, give it a few minutes to settle.

6\. Update Fluidd

7\. Update Mainsail

8\. Update Crowsnest

RESTART the host again

8\. Update klipper

RESTART the host again


If fluidd-config is still showing Invalid / Recover - click on another menu tab, or the home screen, then come back to the update screen.
Then if it still doesn't show, wait about half an hour and do the same, Speculation below:

Speculation:
Every 30 minutes the Gemini backs up files, hence the reason if you don't shut down the machine correctly you can lose up to 30 minutes of changes.
I believe fluidd-config may update from the backup at that time.

This may explain being unable to update it, after many attempts at updating it in the UI and within the linux console it seemed fixed itself once I went to the home screen, then returned to the updates panel.

I think my explanation is correct but without more testing or communication from the developer of the fluidd/mainsail install, I couldn't say for sure.
I hope your updates page looks like this:

![alt text](images/updating/11-done.png?raw=true)
