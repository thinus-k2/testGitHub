# Connect to Sharedo using the K2 JSSP Broker and OAuth Authentication

You can connect to Sharedo using the K2 JSSP broker, along with OAuth authentication. This article illustrates how to configure a K2 JSSP service instance, along with an OAuth resource that gets and returns tokens from Sharedo.

This article contains two parts:
  - ShareDo OAuth Setup
  - K2 OAuth Setup

# Part 1: ShareDo OAuth Setup

Begin by configuring Sharedo for OAuth. This allows K2 to authorize users through tokens sent to Sharedo.

Currently, Sharedo does not have a dedicated administration page for configuring OAuth. You can request OAuth setup by sending an email to support@slicedbread.co.uk. You must provide your Sharedo instance details, along with the redirect URL you need for your OAuth configuration. The redirect URI looks like the following: https://{Your K2 Environment URL}/Identity/token/oauth/2, for example https://void.onk2stable.com/Identity/token/oauth/2

Once Sharedo completes your OAuth configuration, you can obtain the following details from ShareDo’s support team to use in your K2 service instance configuration.  (After you obtain the information below, proceed to Part 2: K2 OAuth Setup.)

  - Authorization endpoint URL
  - Token endpoint URL
  - ClientID
  - Client Secret
  - Refresh token expiration days

# Part 2: K2 OAuth Setup

To configure K2 for OAuth authentication, begin by defining an OAuth Resource type. For more information, see [https://help.k2.com/onlinehelp/Platform/UserGuide/June/default.htm#K2-Management-Site/Authentication/ResourceTypes.htm%3FTocPath%3DAdminister%7CK2%2520Management%7CAuthentication%7COAuth%7C_____1](Resource Types).

Steps to setup OAuth ResourceType:
1. Open **K2 Management Site.**
2. Go to **Authentication>OAuth** nodes. Click **ResourceTypes.**
3. Click the **+New** button in the cental pane toolbar.
4. Specify a **Name**, for example, *ShareDo*
5. Enter a **Description**, for example *ShareDo Resource Type*
6. Do not enter a value for the **Extension** property.
7. For **Refresh Token Expiration Days**, enter the value provided by ShareDo , for example, *15*
8. For **Expiring Warning Days**, enter a value less than the **Refresh Token Expiration Days** value, for example, *10*
9. Enter and **Expiring Message** that explains the token authorization is expiring soon. For example,  
*Ex: Your OAuth token is expiring soon. The token is calculated 15 days after the date you trusted the ShareDo feature. If you don’t use the feature, you must re-trust it to reset your OAuth token. See the details below for the site and refresh token URL.*
11. Enter an **Invalid Message**, for example:
*Ex: The refresh token is expired.  
K2 integration for the below resource has stopped working. Regenerate the token now. Changing the password of the user associated with the token will result in an invalid refresh token until the resource is trusted again. See the details below for the site and refresh token URL.*
12. For **Invalid Message Delay Minutes**, enter 60
13. In the **Usage** box, enter *Authorization*.
14. Click **OK**.

![Alt text](/OAuthSetup-K2-NewResourceType.jpg?raw=true "OAuth Resource Type")

Next, add resource type parameters for the **ShareDo Resource Type**. Select **ShareDo** in the **Resource Types** list (top section). Scroll down to **Resource Type Parameters** (bottom section).
1. Add the **redirect URI** resource parameter type
   1. Click **+New** in the toolbar.
   2. Confirm **Sharedo** is the **Resource Type**. If not, use the drop-down list to select it. 
   3. For the **Parameter Name**, enter *redirect_uri*
   4. For the **Parameter Description**, enter: *Determines where the response is sent. The value of this parameter must exactly match the redirect URI  values registered with the resource, including the http or https schemes, the same case, trailing '/' and no additional trailing spaces).*
   5. Check the the URL Encode box
   6. For the **Refresh Default Value**, check the **Authorization Request** and **Token Request** boxes. Click **OK**.
2. Add the **grant type** resource parameter type
   1. Click **+New** in the toolbar.
   2. Confirm **Sharedo** is the **Resource Type**. If not, use the drop-down list to select it.
   3. For the **Parameter Name**, enter *grant_type*.
   4. For the **Parameter Description**, enter: *As defined in the OAuth 2.0 specification, this field must contain a value of {authorization_code | refresh_token}. (KL – is there an example you can give for clarity?)*
   5. Do not enter a value for **Authorization Default Value**
   6. For the **Token Default Value**, enter: *authorization_code*
   7. For **Refresh Default Value**, enter *refresh_token*. Check the **Token Request** and **Refresh Request** checkboxes.
   8. Click **OK**
3. Add the **scope** resource parameter type
   1. Click **+New** in the toolbar
   2. Confirm Sharedo is the Resource Type. If not, use the drop-down list to select it.
   3. For the **Parameter Name**, enter *scope*.
   4. For the **Parameter Description**, enter *Indicates the access your application is requesting. The values passed in this parameter inform the consent page shown to the user. There is an inverse relationship between the number of permissions requested and the likelihood of obtaining user consent.*
   5. For the **Authorization Default Value**, enter: *sharedo offline_access*.
   6. For the **Refresh Default Value**, check the **Authorization Request** and **Authorization Response** boxes.
   7. Click **OK**.
4. Add the **response type** resource parameter type.
   1. Click **+New** in the toolbar.
   2. Confirm **Sharedo** is the **Resource Type**. If not, use the drop-down list to select it.
   3. For the **Parameter Name**, enter *response_type*.
   4. For the **Parameter Description**, enter *Determines if the OAuth 2.0 endpoint returns an authorization code. For web server applications, a value of code should be used.*
   5. For the **Refresh Default Value**, check the **Authorization Request** box.
   6. Click **OK**.
5. Add the **client ID** resource parameter type.
   1. Click **+New** in the toolbar.
   2. Confirm **Sharedo** is the **Resource Type**. If not, use the drop-down list to select it.
   3. For the **Parameter Name**, enter *client_id*.
   4. For the **Parameter Description**, enter *Indicates the client that is making the request. The value passed in this parameter must exactly match the value registered.*
   5. For the **Refresh Default Value**, check the **Authorization Request**, **Token Request**, and **Refresh Request** boxes.
   6. Click **OK**.
6. Add the **client secret** resource parameter type.
   1. Click **+New** in the toolbar.
   2. Confirm **Sharedo** is the **Resource Type**. If not, use the drop-down list to select it.
   3. For the **Parameter Name**, enter *client_secret*.
   4. For the *Parameter Description**, enter *The client secret obtained during application registration.
   5. For the **Refresh Default Value**, check the **Token Request** and **Refresh Request** boxes.
   6. Click **OK**.

*An OAuth Resource Type Configuration*  
![Alt text](/OAuthSetup-K2-ResourceType.jpg?raw=true "OAuth Resource Type")

The next step is to add an OAuth resource using the values provided by Sharedo. (See **Part 1: Sharedo OAuth Setup** for reference if necessary.)
1. From the **K2 Management Site**, expand the **Authentication > OAuth nodes**. Click **Resources**. 
2. Click **+New** in the central pane toolbar.
3. Enter a value for the **Resource Name**, for example, *Sharedo*.
4. For the **Resource Type**, use the drop-down to select **Sharedo**. (This is the resource type you just created in the previous steps.)
5. For the **Authorization Endpoint URL**, enter the value provided by **Sharedo**.
6. For the **Token Endpoint**, enter the value provided by **Sharedo**.
7. For the Metadata Endpoint, check the **Use Host Server Authorization Endpoint** box.
8. Click **OK**.

Finally, configure the parameters for the new resource.
1. Locate the **Resource Parameters** section. Select **redirect_uri** and click **Edit**. 
   1. Enter the redirect URI you provided Sharedo (in Part 1) into the **Authorization Value**, **Token Value**, and **Refresh Value** text boxes. The URL looks similar to this example (but will be different for your environment):
https://void.onk2stable.com/Identity/token/oauth/2.
   2. Click **OK**.
2. Select **grant_type** (still in the **Resource Parameters** section) and click **Edit**.
   1. For **Token Value**, enter *authorization_code*.
   2. For **Refresh Value**, enter *refresh_token*.
   3. Click **OK**.
3. Select **scope** and click **Edit**.
   1. For **Authorization Value**, enter *sharedo offline_access*.
   2. Click **OK**.
4. Select **response_type** and click **Edit**.
   1. For **Authorization Value**, enter *code*.
   2. Click **OK**.
5. Select **client_id** and click **Edit**.
   1. For **Authorization Value**, **Token Value**, and **Refresh Value**, enter the **ClientID** provided by **Sharedo** (see Part 1).
   2. Click **OK**.
6. Select **client_secret** and click **Edit**.
   1. For the **Token Value** and **Refresh Value**, enter the **Client Secret** provided by **Sharedo** (see Part 1).
   2. Click **OK**.


*An OAuth Resource configuration*  
![Alt text](/OAuthSetup-K2-Resource.jpg?raw=true "OAuth Resource Type")


When you register the service instance, select **OAuth** for the authentication mode and the resource that was created for the **OAuth Resource Name**. You may see a login prompt if you do not have a token assigned to you yet.
