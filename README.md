# LetsDefend: Possible SQL Injection Payload Detected

* **Incident Response:**

This will be my walkthrough of completing an investigation of a possible SQL injection, to further my Blue team knowledge. 

First thing to do is to select: `">>"` to create a case from the information given.

<img width="728" alt="1" src="https://github.com/user-attachments/assets/2e59a96c-929d-497a-b453-df1e87fa4596">

Now once created, start a playbook. This will wlak through and ask questions about this incident to asist in the learning experience.

<img width="742" alt="1" src="https://github.com/user-attachments/assets/afaafda7-2658-4941-88a4-41556b3983e7">

First thing I am asked is to understand what cause the alert to trigger.

<img width="591" alt="Screenshot 2024-11-18 103011" src="https://github.com/user-attachments/assets/7c99e8f1-0bad-4da4-a7e3-660a54fe5f32">


First I want to see what direction the data is traveling.

Inside the monitoring tab, I go to the investigation, and select the drop down. I can see here the data has a 
Source address of: `167.99.169.17` 
Destination address of: `172.16.17.18`

<img width="863" alt="Screenshot 2024-11-18 103240" src="https://github.com/user-attachments/assets/4ba64fa8-3fad-4e48-b7c7-6236c55dff13">

Next in there it shows an alert reason: `"Requested URL Contains OR 1 = 1"`.

But when I look at the request url it doesnt say that, it has a bunch of % and numbers that I cant understand just looking at it. To decode this I used `CyberChef`.

<img width="863" alt="1" src="https://github.com/user-attachments/assets/22ab3ec8-455d-42bb-b7af-22081323f86d">

<img width="952" alt="1" src="https://github.com/user-attachments/assets/9155e15f-a33c-4660-ba48-629e08ea30b0">

This is a very easily recognozable SQL injection attack atempt.

1 = 1 is basically meaning if anything in here is true, send it to me.

After that part is done I select next. The next questions ask to summarize some questions

<img width="501" alt="Screenshot 2024-11-18 104415" src="https://github.com/user-attachments/assets/f6b56a62-85e5-4894-ba30-9e9a7d3c7a95">

This is just general knowlegde to better understand the enviorment of this incident.

First I run the source address through Virus Total & Cisco Talos. This helps provide knowlegde of the attacker.

<img width="921" alt="Screenshot 2024-11-18 105501" src="https://github.com/user-attachments/assets/9c50ef10-37ef-4123-ba06-ed18fc191528">

<img width="929" alt="Screenshot 2024-11-18 105715" src="https://github.com/user-attachments/assets/561acff0-9aba-462f-bb6e-c541fe18481d">

To kind of sum up the direction or flow of this incident is attack is coming from 167.99.169.17 > 172.16.17.18 Known as WebServer1001 > Trying to perform SQL injection on the we server

Now to look deeper into this incident go to the `"Log Managment"` tab.

I am filtering to show all traffic from the destination port containing 167.99.169.17.

This showed about 10 responses, but to show the exact log I am loong for I also incude the SIEM to filter for the raw log to contain the requested url.

<img width="685" alt="1" src="https://github.com/user-attachments/assets/66c5a0a5-047d-4ae4-af68-705426f56c51">

Scrolling down I can find the HTTP response status. I can look that up to provide further information.

<img width="502" alt="1" src="https://github.com/user-attachments/assets/85e4e80f-9f5b-44f7-b0d9-71c23e4104cd">

The HTTP status code 500 is a generic error response. It means that the server encountered an unexpected condition that prevented it from fulfilling the request.

Now I know this attack wasnt asuccessfull.

Clicking next.

First answerable question is:

<img width="499" alt="Screenshot 2024-11-18 111443" src="https://github.com/user-attachments/assets/fde26cdf-dae6-446f-b269-cba3dc995cd6">

I would say this is malicious, luckily nothing was successful. But no normal user would be collecting information this way. And also the IP being pinged on every scanner, are giant red flags.

Next wquestion is the attack type.

<img width="584" alt="Screenshot 2024-11-18 111640" src="https://github.com/user-attachments/assets/586c7f6c-6632-4a0d-8aec-047b6cfd7c70">

Obviously this is a SQL injection being it is being performed in a web application. And the layout of the request URL is indicitive of it being a SQL injection.

Next question is to find if this was planned. I checked the Web Server 1001 processes, network,terminal, and browser history. Also the Email security and didnt find anything confirming it was planned.

Next question is to show the direction of the traffic. This IP is from outside the company/ buisness so it is the Internet -> Company Network

<img width="501" alt="Screenshot 2024-11-18 112739" src="https://github.com/user-attachments/assets/63e090ed-0755-4400-9c82-6f3833b5667a">

Next is if it was successful.

<img width="532" alt="Screenshot 2024-11-18 112927" src="https://github.com/user-attachments/assets/af0f8e89-9936-420d-947d-4026d08a5a7d">

<img width="488" alt="Screenshot 2024-11-18 113153" src="https://github.com/user-attachments/assets/06912f44-2e0d-4d68-a3bd-d6be1c1b1131">

As stated early from the Response status code being 500, this attack wasn't successful.

Next step is to add artifacts, meaning anyhting that relates to the attack.

<img width="495" alt="Screenshot 2024-11-18 113657" src="https://github.com/user-attachments/assets/a174dafc-e9e8-4e64-a5e2-e544a5a40d96">

Next question is asking if this incident needs further escilation. Quick answer is no.

<img width="492" alt="Screenshot 2024-11-18 113712" src="https://github.com/user-attachments/assets/29bcffc8-942d-43ab-92cb-1829950dd391">

After completing that I add a quick summary and notes of what I just performed.

<img width="545" alt="Screenshot 2024-11-18 114420" src="https://github.com/user-attachments/assets/cb9fca79-140f-4c0a-8086-c14b9188ba39">

Now this playbook and incident are done.

<img width="540" alt="Screenshot 2024-11-18 114500" src="https://github.com/user-attachments/assets/21cd2c23-036e-4702-a41c-4aa631dd55a9">

The case is now closed and the investigation can be seen in Case Managment.

<img width="333" alt="Screenshot 2024-11-18 144939" src="https://github.com/user-attachments/assets/d625c781-a92e-40ce-86b2-d8ae363d9a13">

<img width="675" alt="Screenshot 2024-11-18 144947" src="https://github.com/user-attachments/assets/0aba97a1-d932-4e62-a63f-a4c3adb24c3e">










