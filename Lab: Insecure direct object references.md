# Lab Walkthrough: Chat Log Information Disclosure

![Intro](https://github.com/user-attachments/assets/d1680164-783e-4b56-8490-a7c1d8ae24a1)

## Objective
This lab stores user chat logs directly on the server's file system and retrieves them using static URLs. The goal is to find the password for the user **carlos** and log into their account.

---

### 1. Initial Reconnaissance
This is the principal Web page target:

![Target Page](https://github.com/user-attachments/assets/62ad9b05-b032-41ed-a9af-72d39e0243f6)

The lab stores the user chat logs on the server. To start, we need to test the default credentials: `wiener:peter`.

![Login Attempt](https://github.com/user-attachments/assets/1ce077e1-159b-4386-9ca7-c8fbe6446287)

*Result: That didn't work.*

---

### 2. Exploring the Live Chat
In the lab, we have a **"Live chat"** feature:

![Live Chat 1](https://github.com/user-attachments/assets/a8fea696-2526-463a-9b6a-f6654090c0e9)

![Live Chat 2](https://github.com/user-attachments/assets/12a35158-5f0f-4ec9-9c4f-3ed25d2ab76a)

This chat appears to be an example conversation with a support agent. 

---

### 3. Traffic Analysis with Burp Suite
Let's start **Burp Suite** to intercept the traffic and see what's happening under the hood:

![Burp Suite 1](https://github.com/user-attachments/assets/4f85a76e-7cc0-41aa-893d-57d87263048f)

With every request we make, this appears in Burp:

![Burp Suite 2](https://github.com/user-attachments/assets/4aa592ef-4e2f-4831-b448-870cd981829e)

When we download the transcript, we see the following request/response:

![Transcript Request](https://github.com/user-attachments/assets/6181f7e2-f7b0-4cd3-8442-02edfb3de0bb)

![HTTP2 Check](https://github.com/user-attachments/assets/c6dabbdf-ecb7-4067-9135-7e1fe26e6200)

We can observe that the web page is using **HTTP/2**.

![Request Headers](https://github.com/user-attachments/assets/fe1e1a4e-bff2-44a9-9a37-7ddfb708c129)

---

### 4. Exploitation via IDOR/Path Manipulation
Now, we move to the **Repeater** module to test different transcript IDs:

![Repeater Module](https://github.com/user-attachments/assets/8b7fd812-9f5e-4db4-8fd8-b5124574efed)

If I try different transcript numbers (e.g., 5, 6, 7), the server responds with **"No transcript"**:

![No Transcript](https://github.com/user-attachments/assets/e29a2a32-8766-4283-b004-8241070b8820)

However, by manipulating the request, we are able to obtain a transcription belonging to another person (Carlos) containing sensitive info:

![Carlos Transcript](https://github.com/user-attachments/assets/8becab1c-9373-4a19-b1d0-aab4e2ac9286)

---

### 5. Lab Solved
Once we extract the correct credentials from the leaked chat log, we log in and complete the lab:

![Solved](https://github.com/user-attachments/assets/3e2d6641-8930-4868-9fa2-9c53aa9b00c5)

