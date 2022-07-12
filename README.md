# How-To-use-tcpdump-with-Linux

#### Using tcpdump on Ubuntu

## Intro
The `tcpdump` utility exists on Ubuntu by default, though we will confirm your setup below.

*Note: Try to use a modern version of `tcpdump` so you get the new `--print` parameter which allows you to see the packets in realtime even while writing (`-w`) to file.*

## Unrelated - Windows WireShark Training
For Windows users, be sure to watch the great tutorials from this teacher who is amazing. In the first hour you will learn how to setup your interface like a pro.

    #link to youtube playlist
    https://youtu.be/OU-A2EmVrKQ

*Note: Even if you do not use Windows, you might consider presenting captures with WireShark because it looks great.*

## Setup
You can optionally keep everything in one folder, but here we break things out to organize your traces, etc. By using the `~` symbol the new folder is placed in your home directory for the examples below.

    #make directory
    mkdir ~/captures

    #optional
    mkdir ~/describe

    #optional
    mkdir ~/netstats

*Note: The `describe` folder is for optionally adding text files or otherwise echoing notes about any current sessions being sniffed. The `netstats` folder is optional as well, but great for collecting general network info with `netstat` and saving it in a logical place.  We talk about `netstat` later.*

## Check if you have `tcpdump` installed
The following will return `[installed]` if you have the package already.

    sudo apt list tcpdump

## Show the version of `tcpdump`
This is important if you want the new `--print` parameter which is awesome.  It comes with the `tcpdump` that is on `Ubuntu 22.04` and also on `Kali Linux`, but users of `Ubuntu 20.04` will have an older version of `tcpdump` which does not have the `--print` parameter yet.

    tcpdump --version


## Install `tcpdump`
This is included by default, but if somehow you need it, you can install `tcpdump` with the following command.

    sudo apt install tcpdump

*Note: If you already have the package, no action will be taken.*

## Show `tcpdump` location
Optionally, we can use `which` to see the location of `tcpdump`.

    which tcpdump

*Note: Typically you will simply type `tcpdump` and will not need the path when running it.*

## About `sudo`
From here on, we will need `sudo` since this is required for `tcpdump`.

    #optional - warm up your sudo by running sync or some other command
    sudo sync

## Optional - Clear screen
We can press `CTRL+l` to quickly clear the screen, or type the `clear` command.

    #clear the screen
    clear

## Show available interfaces for sniffing
This does not require `sudo` but do not forget to use `sudo` later when actually capturing.

    tcpdump -D

## Example Output

    mike@ubuntu03:~$ tcpdump -D
    1.enp3s0 [Up, Running]
    2.lo [Up, Running, Loopback]
    3.any (Pseudo-device that captures on all interfaces) [Up, Running]
    4.virbr0 [Up]
    5.bluetooth-monitor (Bluetooth Linux Monitor) [none]
    6.nflog (Linux netfilter log (NFLOG) interface) [none]
    7.nfqueue (Linux netfilter queue (NFQUEUE) interface) [none]
    8.wlp4s0 [none]
    9.bluetooth0 (Bluetooth adapter number 0) [none]
    10.virbr0-nic [none]
    mike@ubuntu03:~$

## Run `tcpdump` interactively
Now comes the `sudo` requirement.  If you forget `sudo` the command will simply not run, so you will notice it.

    sudo tcpdump -i 1

*Note: In the above, the interface parameter (`-i`) specifies value of `1` to tell `tcpdump` to use the ethernet adapter known as `enp3s0`,*

*Tip: Your adapter name may vary compared to the example, so be sure to choose the desired adapter number from your list as outputted from `tcpdump -D`.)*


## Run `tcpdump` and output to file (without date)

    sudo tcpdump -i 1 -w ./capture.pcap

*Note: In the above, a file will be created called "capture" in the current directory `./` with a file extension of `.pcap` (though you can use any extension name you like).*

## Run `tcpdump` and automatically name the file using date/time
This example appends the date/time in `UTC` by using `-u` parameter of `date`).

    sudo tcpdump -i 1 -w ./capture-$(date -u +"%FT%H%MZ").pcap

*Tip: The above date formatting should be fine, but if needed check out [https://unix.stackexchange.com/questions/278939/how-do-you-put-date-and-time-in-a-file-name](https://unix.stackexchange.com/questions/278939/how-do-you-put-date-and-time-in-a-file-name).*

## Viewing Captures
Since `tcpdump` uses the standard format you can simply open in `WireShark` or your with your favorite viewer.

## Refining your captures
There are many paramters to limit the file size and rotate, etc. so be see to check the `man` page.

    man tcpdump

## Optional - Enhancing your terminal experience
If you do not have `tmux` yet, you might like having this for opening multiple terminals with various horizontal and vertical splits.  It takes a bit of getting used to, but nonetheless.

    #installing tmux
    sudo apt install tmux

    #launching
    tmux new

    #interactive help and menu list
    press CTRL+B and then ?

    #more help
    man tmux

    #common commands (after pressing CTRL+B)
    "  (horizonal split)
    %  (vertical split)
    o  (move the cursor "over" one terminal)
    c  (create new window)
    0  switch to first windows
    1  switch to second window

## Optional - Watching Logs
You can optionally watch logs as well, which is great to do on a new system so you understand the normal and expected log entries, etc.

    #follow the logs in realtime
    journalctl -f

    #show all of today's logs merged
    journalctl -m --since=today

    #ignore alerts about blacklisted sites (if you use a hosts boot list)
    journalctl -m --since=today | grep -v '/etc/hosts' |more

## Optional - Using `netstat`
This example writes one time using the current date and time to a directory called netstats (but name as desired for your path).

    mkdir ~/netstats
    netstat > ~/netstats/netstat-$(hostname)-$(date -u +"%FT%H%MZ").txt


## Summary
Using `tcpdump` or `wireshark` to baseline systems is great.  It is also valuable to collect or review other logging such as `journalctl` in Linux or `eventvwr` in Windows.

