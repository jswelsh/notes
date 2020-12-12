##**OAuth**
___

>Google, Authorization protocol, Certificate, token

Sources
- [Google OAuth article](https://www.nylas.com/blog/integrate-google-oauth?gclid=CjwKCAiAiML-BRAAEiwAuWVggqBIYd8syPIAPRWWLMk_LCiGf3L5QjTkerDR-5qUUVHNbZIZ_P0_GBoCTnIQAvD_BwE)
- [javabrains OAuth](https://www.youtube.com/watch?v=t4-416mg6iU)

![OAuth code flow](https://i2.wp.com/blogs.innovationm.com/wp-content/uploads/2019/07/blog-open1.png?resize=768%2C427)

OAuth is about authorization and not authentication. Authorization is asking for permission to do stuff. Authentication is about proving you are the correct person because you know things. OAuth doesn’t pass authentication data between consumers and service providers – but instead acts as an authorization token of sorts.

The common analogy is the valet key to your car. The valet key allows the valet to start and move the car but doesn’t give them access to the trunk or the glove box.

>app developers are increasingly turning to OAuth to authenticate user accounts because it provides much better security through secure delegated access that uses access tokens rather than username and password credentials. 

The vast majority of apps and services have used basic username and password authentication for most of software development history. However, this creates problems when you want to give an app access to data or functionality that’s stored inside another service that requires account authentication. This requires the app to store the user’s password, creating a password anti-pattern because passwords are meant to be protected secrets, meaning they shouldn’t be shared with anyone.

Consider this example: Alice wants to grant Bob access to her house to water her plants while she’s away on vacation. The typical solution is for Alice to give Bob a physical key that unlocks all exterior doors to the house. This poses some problems:

- Bob must be trusted with a master key to the exterior doors, which is a fairly large amount of trust to give someone. What happens if it gets lost or stolen?
- What if Bob wants to throw a giant party against the wishes of Alice? His access is unrestricted, meaning Alice can’t control things like the days or times Bob is allowed to enter the house.
- What if Bob makes a copy of the key, but Alice wants to remove his access when she returns from vacation? Alice would need to change the locks.

Let’s compare this to an authentication scenario where app Alice gives app 
Bob a user’s username and password to let app Bob access data and functionality from app Alice: 

- The security system of app Bob must be trusted to protect the account’s username and password. This significantly increases your user’s threat vectors.
- App Bob is granted complete control over the account for app Alice. There’s no way to restrict what data and functionality app Bob can access.
- The only way to ensure that Bob’s access to the user data is revoked is for the user to change their account password in App Alice.

OAuth solves these problems by allowing the end user of app Bob to interact directly with the identity provider service in app Alice and create a cryptographically-signed token that grants access to specified scopes. This token is given to the app Bob which, in turn, uses it authenticate requests to app Alice made on behalf of the user.  

Here’s how OAuth would operate in our scenario above:

- App Bob prompts the user to login with their account on app Alice, and redirects them to the authorization server for app Alice. App Bob provides a list of scopes that indicate the specific data and functionality app Bob needs to access.
- App Alice prompts the user to consent to the the scopes app Bob requested and generates OAuth credentials.
- App Alice redirects the user back to app Bob, which then initializes the authentication flow to retrieve user access tokens. 