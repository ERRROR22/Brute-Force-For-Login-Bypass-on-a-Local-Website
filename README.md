# Brute-force for Login Bypass on a Local Website

## Description & Backstory
The **A-Z Education** website offers courses covering React Development, Amazon Web Services, and Cybersecurity algorithms. The platform features a login interface allowing users to create accounts to purchase and view educational video content. 

However, A-Z Education does not utilize multi-factor authentication methods (such as One-Time Passwords, security questions, or email verification), leaving its authentication endpoints susceptible to credential testing vulnerabilities.

---

## Brute-Force Analysis
This demonstration highlights how an inadequately protected login endpoint can be analyzed using a list of common credentials. By auditing the application's response behavior to automated requests via network assessment tools like Burp Suite, it is possible to identify valid authentication components based on variations in server response statuses.

### Prerequisites
* Python 3.x
* Flask
* Burp Suite Community or Professional Edition

### Implementation & Testing Steps

1. **Environment Setup** Clone the repository and launch the local Flask web server:
   ```bash
   git clone [https://github.com/ERRROR22/Brute-force-for-login-bypass-on-a-local-website.git](https://github.com/ERRROR22/Brute-force-for-login-bypass-on-a-local-website.git)
   cd Brute-force-for-login-bypass-on-a-local-website
   pip install flask
   python app.py
Intercepting Traffic Launch Burp Suite, configure the embedded browser, and navigate to the local login interface. Enable the Intercept feature within the Proxy tab and submit a dummy credential pair to capture the baseline HTTP POST request.

Configuring the Target Positions Forward the captured transaction to Burp Intruder. Set the attack type to Sniper. Highlight the username field value and configure it as the primary payload position.

Enumerating Usernames Load the provided username.txt wordlist into the Payload settings and execute the test. Monitor the results table for anomalies in response attributes.

Observation: The server behavior reveals explicit state indicators. While non-existent users return a standard error, a valid username triggers a 220 status code accompanied by an "Incorrect admin password" JSON payload, confirming user existence.

Enumerating Passwords Once the valid identifier (admin) is confirmed, update the payload position to target the password field exclusively. Load the passwd.txt dictionary file into the payload configurations and run the attack.

Observation: A matching credential set breaks the error loop, resulting in a distinct redirect response status code (210), signaling authorization to the administrative endpoint.

Remediation & Defensive Practices
To secure authentication endpoints against automated brute-force attempts, implement the following defensive controls within the application layer and network infrastructure:

Account Lockout & Rate Limiting: Enforce strict thresholds on consecutive failed authentication attempts based on both account identifiers and source IP addresses.

Cryptographic Delays: Introduce progressive verification delays (adaptive hashing/throttling) for sequential authentication failures to disrupt automated high-speed submission tools.

Multi-Factor Authentication (MFA): Implement time-based one-time password (TOTP) mechanisms or out-of-band verification loops to ensure possession of a secondary factor.

Contextual Validation Challenges: Use modern CAPTCHA implementations on login routes to differentiate human interactions from programmatic scripts.

Robust Password Policies: Mandate complexity guidelines and validate inputs against known breached credential datasets during user registration.

Project Status & Metadata
Latest Comprehensive Review: May 30, 2026

Framework: Flask (Python)

Core Focus: Authentication Security Auditing & Defensive Engineering

Maintained and Documented by: Ritik Sharma