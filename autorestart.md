### Gensyn Project Monitoring

This repository contains a monitoring script for the rl-swarm project running inside a gensyn screen session. The script automatically detects if the project stops producing output and restarts it.

### 1. Initial Setup

Make sure screen and bash are installed on your system.
Clone the repository (or download the files) and navigate to the project folder.

Start the rl-swarm project in a screen session named gensyn using the following command:
```
screen -S gensyn -dm bash -c 'printf "n\n\n" | bash run_rl_swarm.sh'
```
### 2. Creating the Monitoring Script

Open a new terminal.

Create the monitor script using nano:
```
nano monitor_gensyn.sh
```
Copy and paste the following content into monitor_gensyn.sh:
```
#!/bin/bash
# === Gensyn Monitoring Script ===
# Monitors the 'gensyn' screen session and restarts if no output is detected.

LOG_TMP="/tmp/gensyn_last.log" # File to store the previous snapshot of screen output
SCN="gensyn" # Name of the screen session running the project

# Take initial snapshot of screen output
screen -S "$SCN" -X hardcopy "$LOG_TMP"

echo "Starting Gensyn monitor... Checking activity every 60 seconds."

while true; do
sleep 60 # Wait 60 seconds before checking again

# Take a new snapshot of the screen output
NEW_TMP="/tmp/gensyn_new.log"
screen -S "$SCN" -X hardcopy "$NEW_TMP"

# Compare previous snapshot with the new one
if cmp -s "$LOG_TMP" "$NEW_TMP"; then
# If no changes detected, project is stuck. Restart it.
echo "No output detected in the last minute. Restarting project..."
screen -S "$SCN" -X stuff $'\003' # Send Ctrl+C to stop current process
sleep 1
screen -S "$SCN" -X stuff $'\e[A' # Send Up Arrow to recall previous command
sleep 1
screen -S "$SCN" -X stuff $'\n' # Press Enter to run it again
sleep 1
else
# If output has changed, project is running normally
echo "Activity detected. Project is running normally."
fi

# Update the last snapshot file for the next iteration
mv "$NEW_TMP" "$LOG_TMP"
done
```

**Save and exit nano (Ctrl+O, Enter, Ctrl+X).**

**Make the script executable:**
```
chmod +x monitor_gensyn.sh
```
```
+x monitor_gensyn.sh
```
### 3. Running the Monitor

Open a new screen session called gencheck:
```
screen -S gencheck -dm bash -c './monitor_gensyn.sh'
```
To attach to gencheck screen and see monitor output:
```
screen -r gencheck
```
To detach from the screen without stopping it:
```
Ctrl+A then D
```
