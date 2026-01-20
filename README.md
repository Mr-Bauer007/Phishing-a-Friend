# Phishing-a-Friend
This project describes an ethical phishing scenario.
**Project Overview**
This project simulates a simple spear-phishing campaign using Zphisher. It demonstrates the integration of credential harvesting using cloud-based infrastructure.

**The Background Story**
I had a discussion with a friend, who is a WordPress website admin, where he gave me permission to use him as a target for my hacking experiments. I delayed the attack for some months so that when it does come, it would be when he least expects it. I thought that the most ethical way to approach things is to use social engineering tactics. It is important to note that he is aware of some phishing tactics, and has in fact been a target of many a phishing campaign.

**Methodology**
Unlike a standard broad attack, this experiment utilized Privacy Settings as a filtering mechanism. 

Vector: WhatsApp Status.

Filtering: Restricted viewing permissions to a single specific target.

Psychological Trigger: Curiosity and Perceived Exclusivity. The target believes the content is a general update, reducing the suspicion often associated with direct messages.

Reasoning: I used WhatsAPP as a vector because of my knowledge of the target. I knew he is more exposed to email phishing, Smishing, and Vishing. However, if the link came from a trusted friend's WhatsAPP account (which may have been hacked without his knowledge), he would be more likely to take the bait. Also, I reasoned that if I sent the link directly to him, it would raise some flags. Thus, I adjusted my WhatsAPP settings to allow only him to view my status. This is to ensure that no one else, from whom I haven't sought permission, is caught in the cross-fire. 

**The Technical Chain**

Hosting: Zphisher running on a cPouta instance.

Tunneling: Cloudflare tunnel to provide a valid HTTPS certificate (crucial for mobile link previews).

Obfuscation: URL Shortener used to mask the Cloudflare/Phishing domain and provide a cleaner visual look in the status bar.

Data Capture: Captured the Public IP Address upon the initial GET request.

**Key Findings**

The victim clicked on the link as intended but did not enter his credentials. While credentials were not entered, the click itself provided significant actionable intelligence:

Public IP Address: Reveals the target's general geolocation and ISP/Organization.
User-Agent String: Identifies the target’s device, application, OS version, or browser. 
Behavioral Confirmation: Confirms the target is active and susceptible to clicking links from this specific source.

**Lessons Learned**

This experiment highlighted that even a failed credential harvest results in a successful Reconnaissance phase. By leveraging WhatsApp's status privacy settings, I was able to isolate the target and capture the victim's application metadata and network entry points.. This proves that a link-click is a vulnerability in itself. In the best cases, it tells the attacker where a victim is and what works. In the worst cases, it causes the victim to fall prey to attacks that only require a single click such as the MS Copilot attack "Reprompt" that allows an attacker to exfiltrate the victim's personal chat data.

**Security Recommendations**

* Link Inspection: Avoid clicking links without first knowing where they lead, especially shortened URLs. Hover over links to see where they lead, if using a laptop/desktop. Where this is not possible, as in the case of mobile phones, copy the link and use a malicious link checker.
* Employ Zero Trust Tactics: Never click on links from unknown sources. Do not trust links simply because they were sent by a friend. If possible, call to verify that they are really the ones in control of their social media accounts and that they know exactly where the link leads.
* IP Masking: Use a VPN or paste links on TOR browser to prevent diclosure of your true location to the attacker.
* MFA: Always enable multifactor authentication in all your applications. Favor passkeys over passwords, if that option is available.

**How to Reproduce(Lab Environment)**

- Deploy a Linux instance (Ubuntu, Kali) on Cpouta or run on VMware/Virtualbox
- Clone and run 'zphisher'
```git clone https://github.com/htr-tech/zphisher.git
cd zphisher
./zphisher.sh
```
- Select the application whose login you would like to obtain from the victim (26 for WordPress).
- Select 02 for Cloudflared
- Select 'N' when asked "Do you want to a custom port" and "Do you want to change Mask URL?" (unless you want to).
- Copy the link and use a URL shortner (Many will refuse the link, I used t.ly)
- Go to "status privacy" on Whatsapp and set to "Only share with" the victim.
- Write a convincing reason to pique their interest and get them to click the link.
- Monitor zphisher terminal for the details and credentials or 'cd' into the 'auth' directory on zphisher and 'cat ip.txt' & 'cat username.dat'

**Disclaimer**

This lab is for educational purposes only. Whenever real persons are involved, permission must be obtained.

