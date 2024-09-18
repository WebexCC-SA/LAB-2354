# Creating a Self Service Support Option in an Emergency  

## Story
> It is July 19, 2024 and your IT department is starting their day off with a full call queue of users which are all reporting the same issue as 8.5 million other PC users, their PCs are crashing or will not boot.  Worst of all your IT team cannot remotely access they affected user's PC and must walk them through the remediation steps over the phone.  As fortune would have it, the required steps for remediation were quickly spreading across the internet.  Now you just need to walk your users through the remediation process so they can get back to work. 


### High Level Explanation

## Preconfigured elements
1. Existing main flow for the IT department
2. Business Mours
3. Business Hours Overrides
4. Webhook for sending SMS

## Build

### Add a Get Queue Info node

---

### Add a Menu node


---

### Add a Collect Digits node


---

### Add an HTTP Request node


---


### Add A Play Message node


---

### Add a Disconnect Contact node


---


### Add a Queue Contact node


---


### Add a Play Music node


---

### Publish the flow
 
---

### Map to your Channel

---

## Unit Test the Flow


---


## Import the CrowdStrike_Starter Flow

---

### Replace the Play Message node which we put in once we identified the event with a Menu node

---

### Add a Go To node

---

### Publish the Flow Using the Test Tag

---

## Dev Testing

---

### Publish the flow with the Live Tag

---

## Smoke Test

---




<script src='../assets/load.js'><script>