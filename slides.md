---
theme: seriph
themeConfig:
  primary: "#0a3a8c"
background: /assets/paper-dimmed.png
title: Alarming Alarms
class: text-center
transition: view-transition
mdc: true
---

# Alarming Alarms

## Analysing the Security of Dealer-Installed Car Alarm Systems

Presented by: **Yibo Wei**

Authors:
Jerry Yu, Yibo Wei, Sumanth Rao, Mohak Vaswani, Jefferson Chien,

Christian Dameff, Nishant Bhaskar, Aaron Schulman

---

# Today's talk

- **The dealer-installed car alarm ecosystem**
- The problem
- Evaluating the impact
- Security implications and potential solutions

---

# What is a car alarm?

<v-click>

<img src="/assets/alarm.png" style="height: 80%; margin: auto; display: block;" />

</v-click>

---
layout: statement
---

# Why do people install car alarms? {.inline-block.view-transition-title}

---

# Why do people install car alarms? {.inline-block.view-transition-title}

<v-clicks>

- **Theft prevention**
- **The car dealer gives you an offer**
- **The insurance company gives you a discount**
- **You never know it's in your car**

</v-clicks>

---
layout: statement
---

# Why do car dealers sell you car alarms?

---
layout: image
image: https://www.jalopnik.com/jalopnik/images/4b45da70c568555eb27a06e187c55e09.jpg
---


---
layout: image
image: https://www.freep.com/gcdn/presto/2021/08/12/PDTF/4e2625e9-c070-42d8-a159-562dbffd1a3d-SECONDARY-Turo_081021_ES07.JPG?width=1733&height=1201&fit=crop&format=pjpg&auto=webp
---

---
layout: image-left
image: /assets/alarm.png
backgroundSize: 70%
---

# Kronos has a solution

Kronos is not a real name

<v-clicks>

![](/assets/key-car.png)

![](/assets/phone-alarm-car.png)

</v-clicks>

---

# What an Alarm System Does
<v-clicks depth="2">

- **Panics** when shock detected
- Communicates with the mobile app through **Bluetooth**
- **Cuts off the ignition** when locked by the mobile app
- **The mobile app** allows the user to:
  - Unlock and enable ignition
  - Lock and cut off ignition
  - Locate the car on the map

</v-clicks>

---

# What happens when the dealer sells the car?

<v-click>

The dealer sells the alarm system to you

</v-click>

<v-clicks depth="3" after="+1">

- If you **accept** the alarm:
  - The dealer put the alarm in **User mode**
  - You can use the alarm with your mobile app
  - You pay a **monthly fee** to Kronos
  - You get a **discount on your car insurance** (from the insurance company that owns Kronos)
  - If you don't pay, you cannot log in to the mobile app
- If you **don't accept** the alarm:
  - The dealer put the alarm in **No-Sale mode**
    - The alarm refuses all BLE commands
  - The alarm remains in the car unless you strongly request to remove it
  - You can use the car without the alarm

</v-clicks>

---

# The customer's perspective

Two of the authors in this paper have Kronos alarms in their cars

<v-click>

## Sumanth:

- Purchased the car but refused the alarm
- The dealer deactivated the alarm, put it in **No-Sale mode**

</v-click>

<v-click after="+1">

## Jefferson:

- **Purchased car with the alarm** and paid the monthly fee with insurance discount for a while
- The alarm was in **User mode**
- Stopped paying for the monthly fee
- The alarm was left in **User mode**

</v-click>

---
layout: image
image: /assets/ecosystem.png
backgroundSize: 70%
---

# Ecosystem Overview

---

# Today's talk

- The dealer-installed car alarm ecosystem
- **The problem**
- Evaluating the impact
- Security implications and potential solutions

---

<SlidevVideo autoplay controls>
  <!-- Anything that can go in an HTML video element. -->
  <source src="/assets/hack.mp4" type="video/mp4" />
</SlidevVideo>

---
layout: statement
---

# What's going wrong? {.inline-block.view-transition-title}


---
layout: image
image: /assets/key-in-app.png
backgroundSize: 40%
---

# Kronos uses the same hardcoded key for all alarms

---
layout: image
image: /assets/aaron.png
backgroundSize: 50%
---

---

# Today's talk

- The dealer-installed car alarm ecosystem
- The problem
- **Evaluating the impact**
- Security implications and potential solutions

---

# Evaluating the impact

- How many cars are affected?
- Where are the affected cars?
- What is the attack complexity of this vulnerability?


---

# MAC Addresses are sequentially assigned

WiGLE has great data coverage that allows us to estimate the number of affected cars

<img src="/assets/mac-growth.png" style="height: 80%; margin: auto; display: block;" />

---

# Example of MAC address data

The MAC addresses are for demonstration purposes only, they do not represent real MAC addresses

````md magic-move
```{*|1|1-2|1-3|1-4}
AA:BB:CC:00:00:00
AA:BB:CC:00:00:01
AA:BB:CC:00:00:02
AA:BB:CC:00:00:05
AA:BB:CC:00:00:09
AA:BB:CC:00:80:00
AA:BB:CC:00:80:01
AA:BB:CC:00:80:02
...
```
```{1-6}
AA:BB:CC:00:00:00
AA:BB:CC:00:00:01
AA:BB:CC:00:00:02
  AA:BB:CC:00:00:03
  AA:BB:CC:00:00:04
AA:BB:CC:00:00:05
AA:BB:CC:00:00:09
AA:BB:CC:00:80:00
AA:BB:CC:00:80:01
AA:BB:CC:00:80:02
...
```
```{1-10|10-11}
AA:BB:CC:00:00:00
AA:BB:CC:00:00:01
AA:BB:CC:00:00:02
  AA:BB:CC:00:00:03
  AA:BB:CC:00:00:04
AA:BB:CC:00:00:05
  AA:BB:CC:00:00:06
  AA:BB:CC:00:00:07
  AA:BB:CC:00:00:08
AA:BB:CC:00:00:09
AA:BB:CC:00:80:00
AA:BB:CC:00:80:01
AA:BB:CC:00:80:02
...
```
```{11|12-14|*}
AA:BB:CC:00:00:00
AA:BB:CC:00:00:01
AA:BB:CC:00:00:02
  AA:BB:CC:00:00:03
  AA:BB:CC:00:00:04
AA:BB:CC:00:00:05
  AA:BB:CC:00:00:06
  AA:BB:CC:00:00:07
  AA:BB:CC:00:00:08
AA:BB:CC:00:00:09
# Not assigned
AA:BB:CC:00:80:00
AA:BB:CC:00:80:01
AA:BB:CC:00:80:02
...
```
````

---
layout: image
image: /assets/units.png
backgroundSize: 60%
---

---
layout: image
image: /assets/mac-distance.png
backgroundSize: 60%
---

---
layout: image
image: /assets/units-annotated.png
backgroundSize: 60%
---

---
layout: statement
---

# 1.68 Million


---
layout: image
image: /assets/qt-map.png
backgroundSize: contain
---

---
layout: image
image: /assets/georgia_asia.png
backgroundSize: 60%
---

# You can even track where the stolen cars

---

# Attack Complexity

It depends on the alarm's current mode

<v-click>

### Dealer Mode / User Mode

- The alarm advertises and is connectable **all the time**
- You can use most commands including Lock / Unlock

> If you payed for the alarm, but no longer pay the monthly fee, the alarm will be left in **User mode**

</v-click>

<v-click after="+1">

### No-Sale Mode

- The alarm advertises and is connectable only **when engine is on**
  - The attacker needs to wait for the engine to be on
- All Bluetooth commands **are denied**
  - **Except the CHANGE_MODE command**
- Attacker can change the mode back to Dealer mode when the engine is on 💩

</v-click>

---

# How can we tell what modes these alarms are in?

<v-click>

### Kronos BLE advertisement
- **Mode**
- **Battery Voltage**
- Some less important information

</v-click>

<v-click after="+1">

### We can't simply group by modes
- **No-Sale mode** alarms does not advertise when the engine is off
- Data will be **biased towards Dealer mode** and **User mode** alarms because they always advertise

</v-click>

<v-click after="+1">

### Battery Voltage can tell us if the engine is on

- Engine off: 12.2V - 12.6V
- Engine on: 13.7V - 14.7V
- **We can only consider > 13V to get unbiased distribution of modes**

</v-click>

---
layout: image
image: /assets/modes.png
backgroundSize: 60%
---

---

# Today's talk

- The dealer-installed car alarm ecosystem
- The problem
- Evaluating the impact
- **Security implications and potential solutions**

---
layout: statement
---

# Why not using BLE pairing and bonding? {.inline-block.view-transition-title}

---

# Why not using BLE pairing and bonding? {.inline-block.view-transition-title}

Classic Security vs. Usability problem

<v-clicks depth="2">

## It's ideal for **users**
  - A user has one car and one phone
  - A user uses the car and the phone for years

## It's a nightmare for **dealers**
  - Dealers have a lot of cars and many employees with different phones
  - They want to pair every car with every employee's phone every time

</v-clicks>

---
layout: two-cols
---

# Kronos's answer

They moved the authentication key to a cloud API

<v-clicks>

- Kronos is suprisingly open for communication
- Now only registered users can access the key
- But attackers can still get the key if they register as a user

</v-clicks>

::right::

<img src="/assets/key-on-cloud.png" style="width: 80%; margin: auto; display: block;" />

---

# How could it be done better?

<v-clicks>

- Use **pairing and bonding** for **User mode** with a physical button
- Do not advertise when the alarm is in **User mode**
- Use a **dealer-specific key** instead of hardcoded keys for all alarms
  - The key can be programmed into the alarm upon installation
- Require **physical interaction** for critical operations such as changing modes
  - It can be as simple as long-pressing a button on the alarm
- True **No-Sale mode**
  - The alarm should be removed, ideally
  - If not, the alarm should have a hardware deactivate switch

</v-clicks>

---

# Why this is an impossible issue to fix?

<v-clicks depth="2">

- **Recall?**
  - This is not like a manufacturer recall because the alarm is a third-party add-on
  - Kronos is not responsible due to a contract they signed with dealers
  - There are hundreds of dealers and they don't want to be responsible either
- **Firmware update?**
  - Only available through the mobile app
  - The mobile app is only available for payed users
  - Many people don't even know the alarm is in the car

</v-clicks>

---

# Other case studies

All names are fictional

- Rhodes alarm
  - Uses **400MHz radio remote**
  - **BLE** available as an **add-on**
  - Broadcast temporary session key in the advertisement
  - Requires a dealer-specific ID to authenticate

- Hades alarm (it's actually an older version of the Kronos alarm)
  - Uses **KeeLoq Protocol**
    - Historically widely adopted for remote keyless entry systems
    - Multiple known vulnerabilities
  - Has a **hardware resister that deactivates the alarm**
    - There is a piece of firmware code that checks if the resister is present 💩

---
layout: two-cols
---

<SlidevVideo autoplay class="h-52%">
  <!-- Anything that can go in an HTML video element. -->
  <source src="/assets/cluetooth.mp4" type="video/mp4" />
</SlidevVideo>

::right::

# Future Work

### More alarms

- There exists many other alarms
- Some are cellular-based, which are more secure as they use https

### BLE Scanning

- We built a scanner app and backend infrastructure

### Application analysis

- We can **map BLE advertisement to companion applications** with names, UUIDs, and other identifiers
- We want to study **what information do BLE devices advertise**
  - We believe a lot more devices are advertising things they shouldn't advertise

---
layout: image
image: /assets/scope.png
backgroundSize: 27%
---
