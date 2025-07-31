# 🌐 The OSI (Open Systems Interconnection) Model

The **OSI Model** is a standardised model used to demonstrate the **theory behind computer networking**.  
In practice, real-world networking often uses the **TCP/IP model**, which is simpler, but the OSI model is **easier for initial understanding**.

---

## **📚 OSI Model Layers**

The OSI model consists of **seven layers**:

| Layer # | Name         |
|--------|--------------|
| 7      | Application  |
| 6      | Presentation |
| 5      | Session      |
| 4      | Transport    |
| 3      | Network      |
| 2      | Data Link    |
| 1      | Physical     |

**Mnemonic Example:**  
> *Anxious Pale Shakespeare Treated Nervous Drunks Patiently*  

---

## **🔹 Layer 7 – Application**
- Provides **networking options to programs** running on a computer.
- Interfaces directly with applications to **send and receive data**.
- Data here is passed **down to the Presentation Layer**.

---

## **🔹 Layer 6 – Presentation**
- **Translates data** into a standardised format for the receiver.
- Handles **encryption, compression, and formatting**.
- Passes the processed data **to the Session Layer**.

---

## **🔹 Layer 5 – Session**
- **Establishes, maintains, and terminates** sessions between computers.
- Ensures **unique communication sessions** so multiple connections don’t get mixed up.
- Once the session is created, the data moves **to Layer 4 (Transport)**.

---

## **🔹 Layer 4 – Transport**
- Responsible for **reliable or fast data transmission**.
- Common protocols:
  - **TCP (Transmission Control Protocol):** Reliable, connection‑oriented.
  - **UDP (User Datagram Protocol):** Faster, connectionless.
- Breaks data into **bite‑sized pieces**:
  - TCP → **Segments**
  - UDP → **Datagrams**

---

## **🔹 Layer 3 – Network**
- Responsible for **logical addressing and routing**.
- Determines the **best path to the destination** using IP addresses.
- Works with **IP (v4/v6)** and other routing protocols.

---

## **🔹 Layer 2 – Data Link**
- Handles **physical (MAC) addressing**.
- Adds the **MAC address** of the destination device.
- **Checks data for corruption** upon reception.
- Prepares data in a format suitable for **Layer 1 transmission**.

---

## **🔹 Layer 1 – Physical**
- Deals with **hardware and signals**.
- Converts **binary data to electrical, optical, or radio signals** and vice versa.
- The actual **physical transmission medium** (cables, fiber, Wi‑Fi) lives here.

---

## **💡 Key Points**
- Data flows **from top (Application)** down to **bottom (Physical)** when sending.
- Data flows **from bottom (Physical)** up to **top (Application)** when receiving.
- The OSI model is **conceptual**, but it maps nicely to TCP/IP in practice.

---
