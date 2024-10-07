# Overview

## Learning Objectives

This lab will give you an introduction to crafting call flows which extend the functionality of the Webex Contact Center and integrate them into a seamless experience for your agents and customers.

## Disclaimer

Although the lab design and configuration examples could be used as a reference, for design related questions please contact your representative at Cisco, or a Cisco partner.

## Lab Access

In this lab we will be using Webex for making calls into the Contact Center and for viewing demos which will be shared by the instructor.  We will be using the same account for Admin and Agent activities in the Contact Center.  Your guide will reflect your specific environment variables, including login information, Channels, Queues, Teams, and assigned numbers in the steps of the actual lab as you progress.


> Login: <copy><w class="login">Provided by proctor</w></copy>
> 
> Password: <copy><w class="PW">Provided by proctor</w></copy>
>
> Webex Phone Number: <copy><w class="WxC">Provided by proctor</w></copy>
>
> Assigned Inbound Channel Name: <copy><w class="EPname">Provided by proctor</w></copy>
> 
> Assigned Inbound Channel Number: <copy><w class="EPDN">Provided by proctor</w></copy>
>
> Assigned Queue Name: <copy><w class="Queue">Provided by proctor</w></copy>
>
> Assigned Team name: <copy><w class="Team">Provided by proctor</w></copy>


---


## Getting Started

> Log into Webex on your PC:
>
> - Username: <copy><w class="login">Provided by proctor</w></copy>
> - Password: <copy><w class="PW">Provided by proctor</w></copy>
> 

---
> Log into [Webex Control Hub](https://admin.webex.com){:target="_blank"} in Chrome

> Login: <copy><w class="login">Provided by proctor</w></copy>
> 
> Password: <copy><w class="PW">Provided by proctor</w></copy>

---
### Testing your lab setup
> 1. Launch the [Agent Desktop](https://desktop.wxcc-us1.cisco.com/){:target="_blank"} and log in selecting the Desktop option for your Voice connection.
2. From the Webex App, dial <copy><w class="EPDN">Provided by proctor</w></copy>
      1. You will hear a greeting message which will include the pod number which you have been assigned.
      2. The call will be place in your queue.
      3. You will hear the hold music until the call is answered
3. In the agent desktop, set your status to Available and answer the call.
      1. Confirm that you can hear audio being passed in both directions.
      2. Disconnect the call
      3. Select a Wrap-up Code
      4. Set your status to Meeting
4. In Control Hub, navigate to Contact Center > Flows
      1. Locate and open the flow named: <copy><w class ="POD">yourLabID</w>_Demo_Lab0</copy>
      2. Click on Debug at the bottom of the screen.
      3. Click on the interaction ID
      4. Explore the steps the call took as it traversed the flow.
      5. When you are done you can close the flow.

---

<script src='../assets/load.js'><script>