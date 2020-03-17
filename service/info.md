### systemd

+ Goal：run python code at boot using systemd
+ Environment: Raspbian

#### 0. Locate paths
   + locate the path of python executable file \
     My path of the virtualenv is `/home/pi/env/bin/python`
   + locate the path of python script\
     My path of python script  is `/home/pi/testbed/service/oled_server.py`
   + locate the path of service file\
     My path of service file is `/home/pi/testbed/service/service/oled.service`

#### 1. Create service file
```shell script
vim /home/pi/testbed/service/service/oled.service
```
   (press i to edit)
```
[Unit]
Description=OLED Server
After=network.target

[Service]
Type=simple
WorkingDirectory=/home/pi/testbed/service
ExecStart=/home/pi/env/bin/python /home/pi/testbed/service/oled_server.py
Restart=always

[Install]
WantedBy=multi-user.target
```
(press ESC, :x to save)\
create a soft link 
```
ln -s /home/pi/testbed/service/service/oled.service /lib/systemd/system/
```

Explanation：
+ WorkingDirectory - the directory of program
+ ExecStart - the path of virtualenv，python script
+ Restart=always - restart when crash

#### 2.Enable services
```
sudo chmod 644 /lib/systemd/system/test.service
sudo systemctl daemon-reload
sudo systemctl enable test.service
```

#### 3. Additional Commands (FRI)
+ Check service status
  ```
  sudo systemctl status test.service
  ```
   (Active: active，when success，press q to quit)
+ Stop service
  ```
  systemctl stop test.service
  ```
+ Start service
  ```
  systemctl start test.service
  ```
+ Re-start service
  ```
  systemctl restart test.service
  ```
+ Command to debug service (log)：
  ```
  journalctl -u service-name.service
  journalctl -u service-name.service -b
  journalctl -u service-name
  ```