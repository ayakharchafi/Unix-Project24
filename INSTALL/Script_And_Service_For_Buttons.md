# Create Script for the button that records and takes pictures:
## The examples included are provided from our files, path 
  
 1- nano ~/name_of_your_python_script.py (Ex: finalCode.py)
    a.Use nano if you want to add or modify your file. If not, skip this step and go to the second step.
  
 2- Make your python script executable
    a. chmod +x /path/to/python_script.py (Ex:chmod +x /home/Desktop/finalCode.py)
  
 3- Test your script by Running it manually
    a. ./name_of_script.py (Ex: ./finalCode.py)
    b. If there is an error run the command: python3 /path_to_python_script.py

  4- Configure your script to run automatically once the Raspberry Boots using systemd
    a. sudo nano /etc/systemd/system/name_of_script.service (Ex: sudo nano /etc/systemd/system/finalCode.service)
       - add this to your service:
      
      --------------------------------------------------
      [Unit]
      Description=Run Python Script

      [Service]
      ExecStart=/usr/bin/python3 /path/to/your_script.py
      Restart=always
      User=pi (In our case it is aya, since its the user)
      Group=pi (In our case it is aya, since the user's group)

      [Install]
      WantedBy=multi-user.target
      --------------------------------------------------

      
    b. "Description": This describes your service.
    c. "ExecStart": In this line, you will add the full path to the python executable and the script that you wish to run
    d. "WorkingDirectory": This is from where your script will run.
    e. "StandardOutput" & "StandardError": This will redirect standard out and error to the journal. 
        This is useful when you want to verify if everything is running well because, like a journal, 
        it provides a description of what the service is doing. This is great for debugging.
    f. "Restart=always": This ensures that the script restarts automatically if it crashes.
    g. "User" & "Group": Runs the script as the pi or other user and group
    h. Save and exit the editor: Ctrl + O, Enter, Ctrl + X

  5. Reload your systemd daemon and enable the service:
    a. sudo systemctl daemon-reload
    b. sudo systemctl enable your_script.service (Ex: sudo systemctl finalCode.service)
    c. sudo systemctl start your_script.service (Ex: sudo systemctl start finalCode.service)

  6. Congradulations! You have a script running when your Raspberry Pi boots!



