<html lang="en">
<!--
<head>
    <meta name="google-signin-scope" content="profile email">
    <meta name="google-signin-client_id" content="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com">
    <script src="https://apis.google.com/js/platform.js" async defer></script>
</head>
-->

<body>
	<script src="https://apisgoogle.com/js/api.js">
		gapi.load('auth2', function()
		{
			gapi.auth2.init();
		});
		
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
		}

		function signOut() 
		{
		    var auth2 = gapi.auth2.getAuthInstance();
		    auth2.signOut().then(function () 
		    {
			console.log('User signed out.');
		    });
		}
	</script>
<!--
	<div class="g-signin2" data-onsuccess="onSignIn" data-theme="dark"></div>  
	<a href="#" onclick="signOut();">Sign out</a>
-->

	<script src="https://accounts.google.com/gsi/client" async defer></script>
<!--	
	<div id="g_id_onload" 
		data-client_id="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com" 
		data-ux_mode="redirect" 
		data-login_uri="https://sites.google.com/tlmshk.edu.hk/portal">
	</div>
-->
	<div id="g_id_onload"
		data-client_id="407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com" 
		data-callback="handleCredentialResponse">
	</div>
	
	<table border="0" align="center" cellpadding="5" cellspacing="5">
		<tr>
			<td align="center"><img src="https://drive.google.com/uc?export=view&id=11eLQdbP-AUW8jHxRF9qgTrSU2gTJeX22"></td>
		</tr>
		<tr><td align="center">
			<div class="g_id_signin" 
				data-shape="circle" 
				data-size="large" 
				data-theme="filled_blue" 
				data-type="standard">
			</div>
		</td></tr>
	</table>

	<div class="g_id_signout">Sign Out</div>
</body>
</html>
