![Status](https://img.shields.io/badge/status-active-brightgreen)

# Mastering Wireshark: A Learning Roadmap 🦈

This document is my personal roadmap for mastering Wireshark, the world's most popular network protocol analyzer. I will add my notes to each section as I develop my skills in packet analysis, traffic filtering, and security analysis.

---

## Level 1: The Beginner - The Packet Catcher 🎣

**Goal:** To learn how to capture network traffic and navigate the Wireshark interface comfortably.

### Key Skills to Master:
* **Selecting the Right Interface:** Choosing the correct network interface to listen on (e.g., `Wi-Fi: en0` or `Ethernet`).
* **Starting & Stopping Captures:** Using the capture controls to start and stop the collection of packets.
* **Understanding the UI:** Learning the three main panes:
    1.  **Packet List:** The top pane, a list of all captured packets.
    2.  **Packet Details:** The middle pane, a breakdown of the selected packet's protocols.
    3.  **Packet Bytes:** The bottom pane, the raw data of the packet in hexadecimal.
* **Identifying a TCP Handshake:** Capturing traffic and finding the `SYN`, `SYN/ACK`, `ACK` packets that start a TCP connection.



---

## Level 2: The Intermediate - The Conversation Filterer 🗣️

**Goal:** To learn how to filter out the noise and isolate the specific conversations or protocols you care about. This is the most important skill in Wireshark.

### Key Skills to Master:
* **Display Filters:** Using the filter bar at the top to find specific packets. I will learn to use filters like:
    * `ip.addr == 8.8.8.8` (Show packets to or from this IP)
    * `tcp.port == 443` (Show packets using this TCP port)
    * `http` or `dns` (Show packets of a specific protocol)
    * Combining filters with `&&` (AND) and `||` (OR).
* **Follow TCP/UDP Stream:** Right-clicking a packet and selecting "Follow > TCP Stream" to reassemble the entire conversation in a readable format.



---

## Level 3: The Advanced - The Security Analyst 🔎

**Goal:** To use Wireshark's powerful features to identify suspicious or malicious network activity.

### Key Skills to Master:
* **Identifying Scans:** Recognizing the patterns of an Nmap scan (e.g., one IP sending many SYN packets to different ports on a target).
* **Analyzing Unencrypted Protocols:** Capturing traffic from protocols like FTP, Telnet, and HTTP (basic auth) to find cleartext usernames and passwords.
* **Exporting Objects:** Extracting files, images, or executables from captured HTTP traffic.
* **Expert Information:** Using the "Expert Information" tool (`Analyze > Expert Information`) to let Wireshark point out potential network problems like retransmissions and errors.


---

## Level 4: The Expert - The Command-Line Guru 💻

**Goal:** To use Wireshark's command-line tools for automation, remote capture, and scripting.

### Key Skills to Master:
* **Tshark:** Using `tshark`, the command-line version of Wireshark, to capture and filter traffic directly from the terminal without a GUI.
* **Tcpdump:** Using `tcpdump`, a lightweight and powerful command-line packet sniffer that is installed on most Linux systems by default.
* **Scripting & Automation:** Piping the output of `tshark` or `tcpdump` into other scripts (using Python or Bash) to perform automated analysis.
