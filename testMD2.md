# K2 JSSP Share OAuth Setup

To use the JSSP broker with ShareDo, you need to use OAuth Authentication.
So when you setup the Service Instance you will need to choose an OAuth resource that can get and use token from ShareDo.

There are 2 parts:
  - ShareDo OAuth setup
  - K2 OAuth setup


# Part 1: ShareDo OAuth setup

You will need to get OAuth configured in ShareDo.

Currently they don’t have an admin page where you can do this yourself. So you will need to contact: support@slicedbread.co.uk

You will need to supply then with your ShareDo instance details, and also the Redirect URI they should add on their OAuth configuration.
The Redirect URI would be https://{Your Server Host Address}/Identity/token/oauth/2.

*Example: https://void.onk2stable.com/Identity/token/oauth/2*

You then have to get a few details from them to setup the OAuth resource below:
  - Authorization endpoint URL
  - Token endpoint URL
  - ClientID
  - Client Secret
Once you have those details, you can proceed to Part 2.

# Part 2: K2 OAuth setup

To use OAuth in K2, you will need to setup and OAuth resource.  
Use the values you got when OAuth was setup on ShareDo in the steps below.

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

Now when you Register the Service instance, you can choose OAuth as the Authentication Type and select the ShareDo OAuth resource you created here.  
This might prompt you to login if it does not have a token for you yet.
