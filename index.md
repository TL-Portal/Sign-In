<html lang="en">
<!--
<head>
    <meta name="google-signin-scope" content="profile email">
    <meta name="google-signin-client_id" content="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com">
    <script src="https://apis.google.com/js/platform.js" async defer></script>
</head>
-->

<body>
	<script src="https://apis.google.com/js/platform.js?onload=init" async defer>
        function init() 
        {
            gapi.load('auth2', function() 
            {
            /* Ready. Make a call to gapi.auth2.init or some other API */
            });
            
            gapi.load('auth2', () => 
            {
                auth2 = gapi.auth2.init(
                {
                    client_id: '407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com',
                    fetch_basic_profile: true,
                    ux_mode: redirect,
                    redirect_uri: 'https://sites.google.com/tlmshk.edu.hk/portal'
                });
            
                auth2.signIn().then(() => 
                {
                    var profile = auth2.currentUser.get().getBasicProfile();
                    console.log('Image URL: ' + profile.getImageUrl());
                    console.log('ID: ' + profile.getId());
                    console.log('Full Name: ' + profile.getName());
                    console.log('Given Name: ' + profile.getGivenName());
                    console.log('Family Name: ' + profile.getFamilyName());
                    console.log('Email: ' + profile.getEmail());
					
					document.getElementById(userInfo).value = profile.getName();
                }).catch((error) => 
                {
                    console.error('Google Sign Up or Login Error: ', error)
              });
            });
        }
<!--		
		function signIn(googleUser) 
		{
		    // Useful data for your client-side scripts:
		    var profile = googleUser.getBasicProfile();
		    console.log("ID: " + profile.getId()); // Don't send this directly to your server!
		    console.log('Full Name: ' + profile.getName());
		    console.log('Given Name: ' + profile.getGivenName());
		    console.log('Family Name: ' + profile.getFamilyName());
		    console.log("Image URL: " + profile.getImageUrl());
		    console.log("Email: " + profile.getEmail());

		    // The ID token you need to pass to your backend:
		    var id_token = googleUser.getAuthResponse().id_token;
		    console.log("ID Token: " + id_token);
			
			document.getElementById(userInfo).value = profile.getName();
		}

		function signOut() 
		{
		    var auth2 = gapi.auth2.getAuthInstance();
		    auth2.signOut().then(function () 
		    {
			console.log('User signed out.');
		    });
		}
		
		function handleCredentialResponse(response) 
		{
			// decodeJwtResponse() is a custom function defined by you
			// to decode the credential response.
			const responsePayload = decodeJwtResponse(response.credential);

			console.log("ID: " + responsePayload.sub);
			console.log('Full Name: ' + responsePayload.name);
			console.log('Given Name: ' + responsePayload.given_name);
			console.log('Family Name: ' + responsePayload.family_name);
			console.log("Image URL: " + responsePayload.picture);
			console.log("Email: " + responsePayload.email);
		}
-->
	</script>
<!--
	<div class="g-signin2" data-onsuccess="onSignIn" data-theme="dark"></div>  
	<a href="#" onclick="signOut();">Sign out</a>
-->

	<script src="https://accounts.google.com/gsi/client" async defer></script>

	<div id="g_id_onload" 
		data-client_id="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com" 
        data-context="signin" 
		data-ux_mode="redirect" 
        data-login_uri"https://sites.google.com/tlmshk.edu.hk/portal" 
        data-auto_prompt="false" 
        data-nonce="">
	</div>
	
<!--	
	<div id="g_id_onload" 
		data-client_id="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com" 
        data-context="signin" 
		data-ux_mode="redirect" 
		data-login_uri="https://sites.google.com/tlmshk.edu.hk/portal/" 
        data-auto_prompt="false" 
        data-nonce="">
	</div>
	
	<div id="g_id_onload"
		data-client_id="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com" 
		data-callback="handleCredentialResponse">
	</div>
-->
	<div width="100%">
    	<img src="https://drive.google.com/uc?export=view&id=11eLQdbP-AUW8jHxRF9qgTrSU2gTJeX22" width="210" height="45">
        &nbsp;&nbsp;
    	<input type="text" id="userInfo" name="userInto" value="TESTING" border="0" disabled>
    </div>
    
	<table align="center" border="0">
		<tr>
			<td align="center" cellpadding="20"><img src="https://drive.google.com/uc?export=view&id=11eLQdbP-AUW8jHxRF9qgTrSU2gTJeX22"></td>
		</tr>
		<tr><td align="center" cellpadding="20">
			<div class="g_id_signin" 
				data-text="signin" 
                data-width="150" 
				data-locale="en-GB" 
				data-shape="pill" 
				data-size="large" 
				data-theme="filled_blue" 
                data-logo_alignment="left" 
				data-type="standard">
			</div>
		</td></tr>
	</table>

	<div class="g_id_signout">Sign Out</div>
</body>
</html>
