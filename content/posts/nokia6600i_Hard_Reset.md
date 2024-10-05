---
title: "Nokia6600i Hard Reset without Passcode"
date: 2024-10-05T17:21:18+02:00
draft: false
---
## How to reset Nokio 6600i slide (RM570)

I recently had to factory reset a nokia slide 6600i and did not have access to the passcode.
The default codes 0000 00000 1234 12345 123456 all didnt work. Since there are only shady
flash tools only available for windows and i didnt find non russian firmware to flash i dug deeper.

### How-To

- Power off device
- Remove Battery
- Hold PowerButton (Red Call Decline Button) for 10 seconds
- Power On device and do not input anything after holding the Power Button to startup the device
- Wait 10-15 minutes
- The code should be 12345 now

How this works? I dont really know. I think it discharges the device heavily and the passcode is
lost due to beeing in some sort of semi temporary storage which leads to a default fallback.

Anyways, good luck!
