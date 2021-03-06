# Description
A remote msfconsole written in Python 2.7 to connect to the msfrcpd server of metasploit.
This tool gives you the ability to load modules permanently as daemon on your server like autopwn2.
Although it gives you the ability to remotely use the msfrpcd server it is recommended to use it locally with a ssh or mosh shell because certificate validation is not enabled.

### Features
- Optimized delivery & execution of commands.
- Has all msf commands implemented even future ones. This is possible through the structure of the rpc api.
- Browse through your command history with the up and down arrow key.
- Tab completion for system paths. 
- It feels like the normal msfconsole!


# How does it look like ?
```
[*] Connecting to server:
 Host => myDomain.com,
 Port => 55553,
 User => msf,
 Pwd => ***,
 SSL => True

[+] Successfully connected
[*] Console id: 19
     ,           ,
    /             \
   ((__---,,,---__))
      (_) O O (_)_________
         \ _ /            |\
          o_o \   M S F   | \
               \   _____  |  *
                |||   WW|||
                |||     |||


       =[ metasploit v4.12.22-dev-52b81f3                 ]
+ -- --=[ 1577 exploits - 906 auxiliary - 272 post        ]
+ -- --=[ 455 payloads - 39 encoders - 8 nops             ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]


msf > 
```

# How do I use it ?
Usage: Main.py [options]
```
Options:
  -h, --help            show this help message and exit
  -r RESOURCE, --resource=RESOURCE
                        Path to resource file
  -u USERNAME, --user=USERNAME
                        Username specified on msfrpcd
  -p PASSWORD, --pass=PASSWORD
                        Password specified on msfrpcd
  -s, --ssl             Enable ssl
  -P PORT, --port=PORT  Port to connect to
  -H HOST, --host=HOST  Server ip
  -c, --credentials     Use hardcoded credentials
  -e, --exit            Exit after executing resource script
```
With the -c option you can use the credentials hardcoded into Main.py feel free to change them so that you don't have to use the credential parameters all the time.

With the -r option you specify a resource script to load from your computer into the console.



### Example:
This will load a resource script and use the hardcoded credentials:
```
python Main.py -c -r /root/resource/handler/allHandlers.rc
```
This will log in to the msfrpcd server through command line arguments:
```
python Main.py --ssl --port 55553 --host 127.0.0.1 --user msf --pass msf
```


# How do I install it ?
First you must have metasploit installed. If you cant use the installer because you have no graphical environment or whatever use this guide from rapid7: [Setting Up a Metasploit Development Environment](https://github.com/rapid7/metasploit-framework/wiki/Setting-Up-a-Metasploit-Development-Environment)
This will install all needed dependencies:
```
git clone https://github.com/allfro/pymetasploit.git pymetasploit
cd pymetasploit && sudo python setup.py install
```

Also don't forget to start your msfrpcd server:
```
cd metasploit-framework/
ruby msfrpcd -U msf -P msf -p 55553
```

And its probably a good idea to start and connect to the postgresql database:
By the way change the password in the echo line.
```
sudo update-rc.d postgresql enable
sudo service postgresql start
echo "create database msf;create user msf with password 'password';grant all privileges on database msf to msf;" > createdb_sql.txt
sudo -u postgres /usr/bin/psql < /home/postgres/createdb_sql.txt
```
In Metasploit: 
```
db_connect msf:password@127.0.0.1/msf
```


