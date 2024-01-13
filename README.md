Initial Setup
SSH into Raspberry Pi:

Connect to your Raspberry Pi via SSH.
Navigate to the Project Directory:

cd ~/PiInstallStuff/Prometheus (or the relevant directory).
Create a Virtual Environment (Recommended):

python3 -m venv venv
source venv/bin/activate
Create requirements.txt File:

Include prometheus_client and psutil.
Use nano requirements.txt and add the dependencies.
Install Dependencies:

pip install -r requirements.txt
Download the Python Script:

Use wget or curl to download metrics_exporter.py from your GitHub repository.
Example: wget https://raw.githubusercontent.com/theBullfish/PrometeusClientPi/main/metrics_exporter.py
Systemd Service Setup
Create a Systemd Service File:

sudo nano /etc/systemd/system/prometheus_exporter.service
Add the service configuration, ensuring paths are correct.
Reload and Enable the Service:

sudo systemctl daemon-reload
sudo systemctl enable prometheus_exporter.service
Start the Service and Check Status:

sudo systemctl start prometheus_exporter.service
sudo systemctl status prometheus_exporter.service
Common Issues and Solutions
Service Fails to Start (Path Issues):

Ensure the ExecStart path in the systemd service file correctly points to the Python executable in the virtual environment and the script path.
Common mistake: Using placeholder /path/to/venv/bin/python3 instead of the actual path.
Port Already in Use:

If the script fails to start because the port (e.g., 8000) is already in use:
Identify the process using the port: sudo lsof -i :8000
Stop the process if it's an old instance of the script.
Script Runs in Foreground in Terminal:

Running the script manually (python3 metrics_exporter.py) occupies the terminal. Use & at the end to run in the background, or rely on the systemd service for automatic handling.
Checking Logs for Errors:

If the service fails, check logs for details: sudo journalctl -u prometheus_exporter.service
For recent logs: sudo journalctl -u prometheus_exporter.service --since "1 minute ago"
CPU Temperature Metric Shows 0.0:

The method to fetch CPU temperature might not be compatible with your Raspberry Pi model. Check the script or the method used.
Testing and Verification
Test the Metrics Endpoint:

Access http://<raspberry_pi_ip>:8000/metrics to verify that metrics are being exposed.
Reboot Test:

Reboot the Raspberry Pi (sudo reboot) and check if the service starts automatically.
By following these steps and keeping an eye on the common issues, you should be able to set up the Prometheus exporter successfully on future Raspberry Pis. Remember to adjust paths and configurations according to your specific setup and environment.
