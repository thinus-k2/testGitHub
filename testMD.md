# K2 JSSP Share OAuth Setup

To use the JSSP broker with ShareDo, you need to use OAuth Authentication.
When you setup the Service Instance you will need to choose an OAuth resource that can get and use token from ShareDo.

There are 2 parts:
  - ShareDo OAuth setup
  - K2 OAuth setup


# Part 1: ShareDo OAuth setup

You will need to get OAuth configured in ShareDo. This is needed to allow K2 to generate OAuth token to send with requests to ShareDo and authorize the correct user.

Currently ShareDo does not have an admin page where you can do this yourself. Please send an email to support@slicedbread.co.uk to request OAuth setup and details.

You will need to supply them with your ShareDo instance details, and also the Redirect URI that needs to be added to their OAuth configuration.
The Redirect URI would be https://{Your Server Host Address}/Identity/token/oauth/2.

*Example: https://void.onk2stable.com/Identity/token/oauth/2*

You then have to get a few details from them to setup the OAuth resource below:
  - Authorization endpoint URL
  - Token endpoint URL
  - ClientID
  - Client Secret
Once you have those details, you can proceed to Part 2.

# Part 2: K2 OAuth setup

To use OAuth in K2, you will need to setup and OAuth resource in K2, but before you do that, you need to define a OAuth Resource Type for the OAuth Resource.

For OAuth ResourceType reference: https://help.k2.com/onlinehelp/Platform/UserGuide/June/default.htm#K2-Management-Site/Authentication/ResourceTypes.htm%3FTocPath%3DAdminister%7CK2%2520Management%7CAuthentication%7COAuth%7C_____1

Steps to setup OAuth ResourceType:
1. Open K2 Management Site.
2. Go to Authentication->OAuth->ResourceTypes
3. Click the +New button on the top button bar
4. Specify a Name. *Ex: ShareDo*
5. Type in a description. *Ex: A resource for ShareDo*
6. Leave the Extension blank
7. Set the "Refresh Token Expiration Days" according to the time limits that OAuth was setup in ShareDo. *Ex: 15*
8. Expiring Warning Days should ideally be before the "Refresh Token Expiration Days" Value. *Ex: 10*
9. Provide an Expiring Message.  
*Ex: Your admin OAuth token may be expiring soon. This is calculated 15 days after the date you trusted the ShareDo feature. If you don’t use the feature you must re-trust the feature, and this will reset your OAuth token. See the details below for the site and refresh token URL.*
11. Provide an Invalid Message.  
*Ex: The admin refresh token is expired  
See the details below for the site and refresh token URL.  
K2 integration for the below resource has stopped working. Regenerate the token now. Changing the password of the user associated with the token below will result in an invalid refresh token until the resource is trusted again.*
12. Set the Invalid Message Delay Minutes. *Ex: 60*
13. Type "Authorization" In the Usage textbox.
14. Click OK

Now you need to add Resource Type Parameters.

Steps to add Resource Type Parameters:
1. Select the ShareDo Resource Type you created in previous steps from the list.
2. This should then show a blank list in the bottom Resource Type Parameters list.
3. Add the Redirect URI Resource Parameter Type
   1. Click on the +New button on the button bar of the Resource Type Parameters List
   2. Select the ShareDo Resource Type you created in previous steps in the Resource Type dropdown
   3. In Parameter Name, type : redirect_uri
   4. In Parameter description type: Determines where the response is sent. The value of this parameter must exactly match one of the values registered with the resource (including the http or https schemes, case, and trailing '/').
   5. Tick the URL Encode checkbox
   6. Tick the Authorization Request checkbox
   7. Tick the Token Request checkbox
   8. Click OK
4. Add the Grant Type Resource Parameter Type
   1. Click on the +New button on the button bar of the Resource Type Parameters List
   2. Select the ShareDo Resource Type you created in previous steps in the Resource Type dropdown
   3. In Parameter Name, type : grant_type
   4. In Parameter description type: As defined in the OAuth 2.0 specification, this field must contain a value of {authorization_code | refresh_token}.
   5. Leave the Authorization Default Value blank
   6. In Token Default Value type: authorization_code
   7. In Refresh Default Value type: refresh_token
   8. Tick the Token Request checkbox
   9. Tick the Refresh Request checkbox
   10. Click OK
5. Add the Scope Resource Parameter Type
   1. Click on the +New button on the button bar of the Resource Type Parameters List
   2. Select the ShareDo Resource Type you created in previous steps in the Resource Type dropdown
   3. In Parameter Name, type : scope
   4. In Parameter description type: Indicates the access your application is requesting. The values passed in this parameter inform the consent page shown to the user. There is an inverse relationship between the number of permissions requested and the likelihood of obtaining user consent.
   5. In Authorization Default Value type: sharedo offline_access
   6. Tick the Authorization Request checkbox
   7. Tick the Authorization Response checkbox
   8. Click OK
6. Add the Response Type Resource Parameter Type
   1. Click on the +New button on the button bar of the Resource Type Parameters List
   2. Select the ShareDo Resource Type you created in previous steps in the Resource Type dropdown
   3. In Parameter Name, type : response_type
   4. In Parameter description type: Determines if the OAuth 2.0 endpoint returns an authorization code. For web server applications, a value of code should be used.
   5. Tick the Authorization Request checkbox
   6. Click OK
7. Add the Client ID Resource Parameter Type
   1. Click on the +New button on the button bar of the Resource Type Parameters List
   2. Select the ShareDo Resource Type you created in previous steps in the Resource Type dropdown
   3. In Parameter Name, type : client_id
   4. In Parameter description type: Indicates the client that is making the request. The value passed in this parameter must exactly match the value registered.
   5. Tick the Authorization Request checkbox
   6. Tick the Token Request checkbox
   7. Tick the Refresh Request checkbox
   8. Click OK
8. Add the Client Secret Resource Parameter Type
   1. Click on the +New button on the button bar of the Resource Type Parameters List
   2. Select the ShareDo Resource Type you created in previous steps in the Resource Type dropdown
   3. In Parameter Name, type : client_secret
   4. In Parameter description type: The client secret obtained during application registration.
   5. Tick the Token Request checkbox
   6. Tick the Refresh Request checkbox
   7. Click OK

Example of OAuth Resource Type setup:  
![Alt text](/OAuthSetup-K2-ResourceType.jpg?raw=true "OAuth Resource Type")

Now you need to setup and configure the OAuth Resource with the values you got when OAuth was setup on ShareDo

Steps to setup OAuth Resource:
1. Open K2 Management Site.
2. Go to Authentication->OAuth->Resources
3. Click the +New button on the top button bar
4. Enter a Resource Name. Ex: ShareDo
5. Select the ShareDo Resource Type you created in the previous steps from the dropdown
6. Enter the Authorization Endpoint URL you got during the ShareDo OAuth setup in Part 1
7. Enter the Token Endpoint URL you got during the ShareDo OAuth setup in Part 1
8. Tick the "Use Host Server Authorization Endpoint"
9. Click OK

Now you need to configure the Resource Parameters.

1. In the Resource Parameter List select redirect_uri and click on the Edit button
   1. Type the Redirect URI you used to configure OAuth in ShareDo into the Authorization Value, Token Value and Refresh Value. Ex: https://void.onk2stable.com/Identity/token/oauth/2
   2. Click OK
2. In the Resource Parameter List select grant_type and click on the Edit button
   1. Type "authorization_code" (without quotes) into the Token Value
   2. Type "refresh_token" (without quotes) into the Refresh Value
   3. Click OK
3. In the Resource Parameter List select scope and click on the Edit button
   1. Type "sharedo offline_access" (without quotes) into the Authorization Value
   2. Click OK
4. In the Resource Parameter List select response_type and click on the Edit button
   1. Type "code" (without quotes) into the Authorization Value
   2. Click OK
5. In the Resource Parameter List select client_id and click on the Edit button
   1. Type the ClientID you got when you configured OAuth in ShareDo in Part 1 into the Authorization Value, Token Value and Refresh Value.
   2. Click OK
6. In the Resource Parameter List select client_secret and click on the Edit button
   1. Type the Client Secret you got when you configured OAuth in ShareDo in Part 1 into the Token Value and Refresh Value.
   2. Click OK

Example of OAuth Resource setup:  
![Alt text](/OAuthSetup-K2-Resource.jpg?raw=true "OAuth Resource Type")


Now when you Register the Service instance, you can choose OAuth as the Authentication Type and select the ShareDo OAuth resource you created here.  
This might prompt you to login if it does not have a token for you yet.

