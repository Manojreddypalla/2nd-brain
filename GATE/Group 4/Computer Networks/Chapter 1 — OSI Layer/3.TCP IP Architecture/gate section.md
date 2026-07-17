For **GATE**, here's exactly what you should know—and nothing extra.

# TCP/IP Architecture — GATE Notes

## 1. Definition ⭐

> **TCP/IP is a protocol suite used for communication over the Internet.**

- Not just TCP and IP.
    
- Contains many protocols like HTTP, DNS, TCP, UDP, IP, ICMP, etc.
    

**GATE Trick:** If a question says _"TCP/IP consists only of TCP and IP"_ → **False**.

---

## 2. Developed By ⭐

- Developed for **ARPANET**
    
- Funded by **DARPA (Defense Advanced Research Projects Agency)**
    

**Possible MCQ:**

> TCP/IP was developed by?

✅ DARPA

---

## 3. Number of Layers ⭐⭐⭐

Standard TCP/IP Model:

1. Application
    
2. Transport
    
3. Internet
    
4. Network Access (Link)
    

> Some books show **5 layers** by splitting the Link layer into **Data Link** and **Physical**. GATE generally follows the **4-layer** model unless the question explicitly states otherwise.

---

## 4. Layer Mapping ⭐⭐⭐

|OSI|TCP/IP|
|---|---|
|Application|Application|
|Presentation|Application|
|Session|Application|
|Transport|Transport|
|Network|Internet|
|Data Link|Network Access|
|Physical|Network Access|

**PYQ Favorite:** Which OSI layers are merged into the TCP/IP Application layer?

✅ Application + Presentation + Session

---

## 5. Responsibilities ⭐⭐⭐

|Layer|Responsibility|
|---|---|
|Application|User services|
|Transport|End-to-end (process-to-process) communication|
|Internet|Logical addressing and routing|
|Network Access|Framing and physical transmission|

Know these responsibilities—they're often tested.

---

## 6. Important Protocols ⭐⭐⭐

|Layer|Protocols|
|---|---|
|Application|HTTP, HTTPS, FTP, SMTP, DNS|
|Transport|TCP, UDP|
|Internet|IP, ICMP, IGMP|
|Network Access|Ethernet, Wi-Fi, PPP|

Memorize this table.

---

## 7. PDU (Protocol Data Unit)

Although you'll study this in more detail with encapsulation, remember:

|Layer|PDU|
|---|---|
|Application|Data|
|Transport|Segment (TCP) / Datagram (UDP)|
|Internet|Packet|
|Network Access|Frame|
|Physical|Bits|

This is asked frequently in networking questions.

---

## 8. TCP/IP vs OSI ⭐⭐⭐⭐

|OSI|TCP/IP|
|---|---|
|7 layers|4 layers|
|Reference model|Practical implementation|
|Developed by ISO|Developed by DARPA|
|Defines services, interfaces, and protocols|Focuses mainly on protocols|
|Rarely implemented as-is|Used on the Internet|

Know at least the highlighted differences.

---

# PYQ Checklist ✅

You should be able to answer:

- Which layer performs routing?
    
    - ✅ Internet Layer
        
- Which layer provides end-to-end communication?
    
    - ✅ Transport Layer
        
- Which protocols belong to the Transport layer?
    
    - ✅ TCP, UDP
        
- Which OSI layers are merged in TCP/IP?
    
    - ✅ Application, Presentation, Session
        
- TCP/IP is:
    
    - ✅ A protocol suite
        
- Developed by:
    
    - ✅ DARPA
        
- Standard TCP/IP has:
    
    - ✅ 4 layers
        

---

# Expected Weight in GATE

- **Difficulty:** ⭐☆☆☆☆ (Easy)
    
- **Question Type:** Mostly conceptual MCQs or matching questions.
    
- **Time to Revise:** 5–10 minutes before the exam.
    

Don't spend hours here. The marks in Computer Networks usually come from deeper topics like **CRC, Hamming Code, Sliding Window, Ethernet, Subnetting, Routing Algorithms, TCP Congestion Control, and DNS**. This architecture topic is your foundation; know it well, then move on.