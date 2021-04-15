+++
date = "2020-10-02T17:09:14-05:00"
draft = false
title = "Sniffing in Go."
slug = 'sniffing-in-go'

+++

## What is sniffing ?

Sniffing is a process of monitoring and capturing all data packets passing through given network. Sniffers are used by network/system administrator to monitor and troubleshoot network traffic. Attackers use sniffers to capture data packets containing sensitive information such as password, account information etc.

So, in layman terms: caputring packets from network traffic for leverage.

Wireless traffic is particularly vulnerable to sniffing because all the packets are broadcast through the air instead of through Ethernet, where physical access is required for the wire to intercept traffic. Providing free wireless internet with no encryption is very common for coffee shops and other venues. This is convenient for guests, but puts your information at risk. If a venue offers encrypted wireless internet, it is not automatically safer. If the password is posted somewhere on the wall, or it is given out freely, then anyone with the password can decrypt the wireless traffic. A popular technique to add security to guest wireless is with a captured portal. Captured portals require the user to authenticate in some way, even as a guest, and then their session is segmented with separate encryption so that others cannot decrypt it.

Wireless access points that offer completely unencrypted traffic must be used carefully. If you connect to a site where sensitive information is passed, be sure that it is using HTTPS so that your data is encrypted between you and the web server you are visiting. VPN connections also offer encrypted tunnels over unencrypted channels.

Some websites are built by unaware or negligent programmers who do not implement SSL on their servers. Some websites only encrypt the login page so that your password is secure, but subsequently pass the session cookie in plaintext. This means that anyone who can pick up the wireless traffic can see the session cookie and use it to impersonate the victim to the web server. The web server will treat the attacker as if they were logged in as the victim. The attacker never learns the password but doesn't need it as long as the session remains active.

Some websites do not have an expiration date on sessions, and they will remain active until explicitly logged out. Mobile applications are particularly vulnerable to this because users very rarely log out and log back into mobile apps. Closing an app and re-opening it does not necessarily create a new session.

Following is an example app to capture packets in go. This will open a network device for live capture and print detail of each packet received. It will continue to run unless the interuppt with `Ctrl+C`
```go
package main

import (
   "fmt"
   "github.com/google/gopacket"
   "github.com/google/gopacket/pcap"
   "log"
   "time"
)

var (
   device            = "eth0"
   snapshotLen int32 = 1024
   promiscuous       = false
   err         error
   timeout     = 30 * time.Second
   handle      *pcap.Handle
)

func main() {
   // Open device
   handle, err = pcap.OpenLive(device, snapshotLen, promiscuous,  
      timeout)
   if err != nil {
      log.Fatal(err)
   }
   defer handle.Close()

   // Use the handle as a packet source to process all packets
   packetSource := gopacket.NewPacketSource(handle, handle.LinkType())
   for packet := range packetSource.Packets() {
      // Process packet here
      fmt.Println(packet)
   }
}
```
You can run it on your local or on the go [playground](https://play.golang.org/) and see how it goes for you ?