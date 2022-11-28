import socket
import threading
import os

portList = []
ips = []

def ipCheck(ip):
    if "TTL" in os.popen('ping -n 1 '+ip).readlines()[2]:
        ips.append(ip)

def portCheck(ip, port):
    try:
        s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
        s.connect((ip,port))
        portList.append(port)
    except:
        pass

def ipScan(subnet):
    mask = ".".join(subnet.split('.')[0:3])+'.'
    for i in range(255):
        ip = mask + str(i)
        thread = threading.Thread(target=ipCheck, args=[ip])
        thread.start()
    print(ips)

def portScan(ip):
    portBefore = int(input('Введите порт от 0 до: '))
    for i in range(portBefore):
        thread = threading.Thread(target=portCheck,args=(ip,i))
        thread.start()
    print(f"\n\n{ip}")
    print(portList)

def main():
    ip24 = input('Введите сеть с маской /24: ')
    ipScan(ip24)
    for i in range(len(ips)):
        print(f"{i}: {ips[i]}")
    target = input('Введите номер ip адреса для скана портов: ')
    portScan(ips[int(target)])

if __name__ == "__main__":
    main()
