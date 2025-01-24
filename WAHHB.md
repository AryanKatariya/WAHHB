
# Web Application (In)security


### **The Evolution of Web Applications**

In the early days of the Internet, the World Wide Web consisted only of web sites. These were essentially information repositories containing static documents.

Web browsers were invented as a means of retrieving and displaying those documents.

The flow of interesting information was one-way, from server to browser. Most sites did not authenticate users, because there was no need to. Each user was treated in the same way and was presented with the same information.

Any security threats arising from hosting a website were related largely to vulnerabilities in web server software

If an attacker compromised a web server, he usually would not gain access to any sensitive information, because the information held on the server was already open to public view.

Rather, an attacker typically would modify the fi les on the server to deface the web site’s contents or use the server’s storage and bandwidth to distribute “warez.”

![[Pasted image 20250123111537.png]]


Today, the World Wide Web is almost unrecognizable from its earlier form.

The majority of sites on the web are in fact applications. They are highly functional and rely on two-way flow of information between the server and browser.

They support registration and login, financial transactions, search, and the authoring of content by users.

The content presented to users is generated dynamically on the fl y and is often tailored to each specific user.

Applications that are accessed using a computer browser increasingly overlap with mobile applications that are accessed using a smartphone or tablet.

Most mobile applications employ either a browser or a customized client that uses HTTP-based APIs to communicate with the server.

The most serious attacks against web applications are those that expose sensitive data or gain unrestricted access to the back-end systems on which the application is running

For many organizations, however, any attack that causes system downtime is a critical event. Application-level denial-of-service attacks can be used to achieve the same results as traditional resource exhaustion attacks against infrastructure.

*Some common categories of vulnerability:*
- Broken authentication (62%) — This category of vulnerability encompasses various defects within the application’s login mechanism, which may enable an attacker to guess weak passwords, launch a brute-force attack, or bypass the login. 

- Broken access controls (71%) — This involves cases where the application fails to properly protect access to its data and functionality, potentially enabling an attacker to view other users’ sensitive data held on the server or carry out privileged actions.

- SQL injection (32%) — This vulnerability enables an attacker to submit crafted input to interfere with the application’s interaction with back-end databases. An attacker may be able to retrieve arbitrary data from the application, interfere with its logic, or execute commands on the database server itself.

- Cross-site scripting (94%) — This vulnerability enables an attacker to target other users of the application, potentially gaining access to their data, performing unauthorized actions on their behalf, or carrying out other attacks against them.

- Information leakage (78%) — This involves cases where an application divulges sensitive information that is of use to an attacker in developing an assault against the application, through defective error handling or other behaviour. 

- Cross-site request forgery (92%) — This flaw means that application users can be induced to perform unintended actions on the application within their user context and privilege level. The vulnerability allows a malicious web site visited by the victim user to interact with the application to perform actions that the user did not intend.

The Core Security Problem: Users Can Submit Arbitrary Input:
- Users can interfere with any piece of data transmitted between the client and the server, including request parameters, cookies, and HTTP headers. Any security controls implemented on the client side, such as input validation checks, can be easily circumvented.

- Users can send requests in any sequence and can submit parameters at a different stage than the application expects, more than once, or not at all. Any assumption developers make about how users will interact with the application may be violated.

- Users are not restricted to using only a web browser to access the application. Numerous widely available tools operate alongside, or independently of, a browser to help attack web applications. These tools can make requests that no browser would ordinarily make and can generate huge numbers of requests quickly to find and exploit problems.

The majority of attacks against web applications involve sending input to the server that is crafted to cause some event that was not expected or desired by the application’s designer.
- Changing the price of a product transmitted in a hidden HTML form fi eld to fraudulently purchase the product for a cheaper amount 

- Modifying a session token transmitted in an HTTP cookie to hijack the session of another authenticated user 

- Removing certain parameters that normally are submitted to exploit a logic flaw in the application’s processing 

- Altering some input that will be processed by a back-end database to inject a malicious database query and access sensitive data

---
### Core Defence Mechanisms

The defence mechanisms employed by web applications comprise the following core elements:
- Handling user access to the application’s data and functionality to prevent users from gaining unauthorized access 
- Handling user input to the application’s functions to prevent malformed input from causing undesirable behaviour 
- Handling attackers to ensure that the application behaves appropriately when being directly targeted, taking suitable defensive and offensive measures to frustrate the attacker 
- Managing the application itself by enabling administrators to monitor its activities and configure its functionality

#### **Handling User Access**
Most web applications handle access using a trio of interrelated security mechanisms: 
- Authentication 
- Session management 
- Access control

Because of their interdependencies, the overall security provided by the mechanisms is only as strong as the weakest link in the chain.

##### Authentication
Authenticating a user involves establishing that the user is in fact who he claims to be.

When attacking a web application, invest a significant amount of attention to the various authentication-related functions it contains.
###### Modern Advancements
- **Multi-Factor Authentication (MFA):**
    - Use of multiple verification methods (e.g., passwords, SMS, biometrics) is now a common security layer.
- **Passwordless Authentication:**
    - Technologies like WebAuthn and FIDO2 allow for authentication via biometrics or device-based tokens instead of passwords.
- **OAuth and OpenID Connect (OIDC):**
    - OAuth 2.0 and OIDC have become standard protocols for delegated and federated authentication.
- **Biometric Authentication:**
    - Fingerprint, facial recognition, and other biometric mechanisms are increasingly integrated into web applications.
- **Single Sign-On (SSO):**
    - Widely used to allow users to log in once and access multiple applications seamlessly.

##### Session Management
After successfully logging in to the application, the user accesses various pages and functions, making a series of HTTP requests from his browser.
At the same time, the application receives countless other requests from different users, some of whom are authenticated and some of whom are anonymous.
To enforce effective access control, the application needs a way to identify and process the series of requests that originate from each unique user.

Virtually all web applications meet this requirement by creating a session for each user and issuing the user a token that identifies the session. The session itself is a set of data structures held on the server that track the state of the user’s interaction with the application.
When a user receives a token, the browser automatically submits it back to the server in each subsequent HTTP request, enabling the application to associate the request with that user.

HTTP cookies are the standard method for transmitting session tokens, although many applications use hidden form fi elds or the URL query string for this purpose.

In terms of attack surface, the majority of attacks against it seek to compromise the tokens issued to other users.
If this is possible, an attacker can masquerade as the victim user and use the application just as if he had actually authenticated as that user.
###### Modern Advancements
- **JWT (JSON Web Tokens):**
    - Commonly used for stateless session management, where the token contains the session information.
- **SameSite Cookies:**
    - Introduced to mitigate CSRF (Cross-Site Request Forgery) attacks by restricting how cookies are sent with cross-site requests.
- **Secure Storage of Tokens:**
    - LocalStorage, SessionStorage, and cookies are used for token storage, but cookies with `HttpOnly` and `Secure` flags are preferred for sensitive tokens.
- **Session Expiry and Rotation:**
    - Regular session expiry and token rotation are implemented to reduce the risk of session hijacking.

##### Access Control
The access control mechanism usually needs to implement some fi ne-grained logic, with different considerations being relevant to different areas of the application and different types of functionality.

Developers often make flawed assumptions about how users will interact with the application and frequently make oversights by omitting access control checks from some application functions.

Probing for these vulnerabilities is often laborious, because essentially the same checks need to be repeated for each item of functionality. Because of the prevalence of access control flaws, however, this effort is always a worthwhile investment when you are attacking a web application.

###### Modern Advancements
- **Role-Based Access Control (RBAC):**
    - Still widely used but now often complemented by Attribute-Based Access Control (ABAC), where policies are more dynamic and context-aware.
- **Policy as Code:**
    - Tools like Open Policy Agent (OPA) allow for centralized, programmatic enforcement of access policies.
- **Zero Trust Architecture:**
    - Assumes no user or device is inherently trusted, and access is continuously verified.


#### **Handling User Input**

“Input validation” is often cited as the necessary defence against Input-based attacks.
However, no single protective mechanism can be employed everywhere, and defending against malicious input is often not as straightforward as it sounds.

