Scapy Robot Framework Library
==============================

## Introduction
Scapy Robot Framework Library provide keywords to do network operations through [Scapy](https://github.com/secdev/scapy). 

## Functionality
* Expose scapy layers as keywords to [Robot Framework](http://robotframework.org/)
* Expose functions in scapy.all as keywords to [Robot Framework](http://robotframework.org/)
* Create/send/receive packets
* Open/Save pcap file (not supported yet)

## Keyword Documentation
[Keywords](http://rainmanwy.github.io/robotframework-ScapyLibrary/doc/ScapyLibrary.html)

## Generate Scapy Layer Documentation
* User could generate scapy layer documentation with below command.
```
python -m ScapyLibrary.layerDoc Scapy.html
```


## Example
### ICMP Ping Example
```
*** Settings ***
Library    ScapyLibrary

*** Test Cases ***
Send And Receive, Return Reply
    Create ICMP Packet
    Send And Get Reply

*** Keywords ***
Create ICMP Packet
    ${ip}     IP      dst=192.168.1.1
    ${icmp}    ICMP
    ${PACKET}    Compose Packet    ${ip}    ${icmp}
    Log Packets    ${PACKET}
    Set Test Variable    ${PACKET}

Send And Get Reply
    ${reply}    Send And Receive At Layer3    ${PACKET}    timeout=${10}
    Log Packets    ${reply[0]}
    ${reply}       Send And Receive At Layer3    ${PACKET}    timeout=${10}    return_send=${True}
    Log Packets    ${reply[0][0]}
    Log Packets    ${reply[0][1]}
    ${reply}    ${unanswered}    Send And Receive At Layer3    ${PACKET}    timeout=${10}    return_send=${True}    return_unanswer=${True}
    Log Packets    ${reply[0][0]}
    Log Packets    ${reply[0][1]}
    Should Be Equal As Integers    ${unanswered.__len__()}    0
```

