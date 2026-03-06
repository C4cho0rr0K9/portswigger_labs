
# Lab: Horizontal Privilege Escalation via User Account Page

## 🎯 Target Objective
The goal is simple (for some of us): exploit a horizontal privilege escalation vulnerability to snatch **Carlos's API key**. 

> **Note:** We aren't brute-forcing our way through the front door like amateurs. We’re going to be a bit more... elegant.

---

## 🛠 Credentials
* **User:** `wiener`
* **Password:** `peter`

---

## 🕵️‍♂️ The Recon (The "Don't Be Obvious" Method)

While the script kiddies are busy clicking buttons, we're going to keep our hands clean.

### 1. Passive Observation
First rule of Fight Club: **Don't activate Intercept Proxy yet.** We don't want to hang the browser while we're just looking around. Let the traffic flow naturally while Burp Suite sits in the corner taking notes.

### 2. HTTP History: Your Real Best Friend
Forget your social life; the **HTTP History** tab in Burp is your only true friend here. 

* Log in with the `wiener` credentials.
* Navigate to your account page.
* Check the history for the request that loads the user profile.



---

## 🔓 The Exploit

1.  **Locate the Request:** Find the GET request that fetches the account details (e.g., `/my-account?id=wiener`).
2.  **The Switch:** Right-click that request and send it to **Repeater**.
3.  **Horizontal Hop:** Change the `id` parameter from `wiener` to `carlos`.
4.  **Profit:** Send the request. If the server is as lazy as the devs who built it, it'll hand over Carlos's private data, including that sweet, sweet API key.

---

## 📸 Proof of Concept
Here is the evidence of the crime:

<img width="868" height="427" alt="Image" src="https://github.com/user-attachments/assets/e1115927-edbb-4c50-84ca-2303bfdec192" />

---
*Stay l33t.* 💻
