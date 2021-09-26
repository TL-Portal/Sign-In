<html>
<head>
    <title>Sign In TL-Portal</title>
    <meta charset='utf-8' />

	<style>    
		.button
		{
			font-family: "Trebuchet MS", "Arial Narrow", Helvetica, sans-serif;
			font-size: 1em;
			width: 100px;
			height: 30px;
			background-color: #eebb00; /* orange */
			color: white;
			border: none;
			border-radius: 38%;
			padding: 3px 3px;
			margin: 3px 3px;
			cursor: pointer;
			text-align: center;
			text-decoration: none;
			display: inline-block;
			transition-duration: 0.3s;
			-webkit-transition-duration: 0.3s; /* Safari */
		}
		.button:hover
		{
			background-color: #ccff66;
			color: #4caf50; /* green */
			box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
		}
		.buttonAlarm
		{
			font-size: 1.2em;
			width: 120px;
			height: 30px;
			background-color: #ff5500;
			color: yellow;
			-webkit-animation: glowing 2500ms infinite;
			-moz-animation: glowing 2500ms infinite;
			-o-animation: glowing 2500ms infinite;
			animation: glowing 2500ms infinite;
		}
		.buttonAlarm:hover
		{
			color: black;
		}
		@-webkit-keyframes glowing 
		{
			0% { background-color: #ff5500; -webkit-box-shadow: 0 0 3px #ff5500; }
			50% { background-color: #eebb00; -webkit-box-shadow: 0 0 20px #eebb00; }
			100% { background-color: #ff5500; -webkit-box-shadow: 0 0 3px #ff5500; }
		}
		@-moz-keyframes glowing 
		{
			0% { background-color: #ff5500; -moz-box-shadow: 0 0 3px #ff5500; }
			50% { background-color: #eebb00; -moz-box-shadow: 0 0 20px #eebb00; }
			100% { background-color: #ff5500; -moz-box-shadow: 0 0 3px #ff5500; }
		}
		@-o-keyframes glowing 
		{
			0% { background-color: #ff5500; box-shadow: 0 0 3px #ff5500; }
			50% { background-color: #eebb00; box-shadow: 0 0 20px #eebb00; }
			100% { background-color: #ff5500; box-shadow: 0 0 3px #ff5500; }
		}
		@keyframes glowing 
		{
			0% { background-color: #ff5500; box-shadow: 0 0 3px #ff5500; }
			50% { background-color: #eebb00; box-shadow: 0 0 20px #eebb00; }
			100% { background-color: #ff5500; box-shadow: 0 0 3px #ff5500; }
		}
	</style>

</head>
<body>
    <div id="signin" style="display:block; margin-top:30px; margin-left:0px; width:100%">
        <p align="center">
        	<!-- School Badge -->
            <img src="https://drive.google.com/uc?export=view&id=1pwPSIdBLY4oNaFOsfVAF6kgMH8C-1zOs" height="35%">
            <br>
        	<!-- TL-Portal -->
        	<img src="https://drive.google.com/uc?export=view&id=1lF8j8LglJ5RP6c1v0MYf5z1_lfT6hMHj" height="12%">
            <br><br>
            <!--Add buttons to initiate auth sequence and sign in-->
            <button class="button" id="signin-button" onClick="signIn()">Sign In</button>
        </p>
	</div>
    
	<div id="signed" style="display:none; margin-top:30px; margin-left:0px; width:100%">
        <p align="center">
        	<!-- User Info -->
            <font face="Arial">
                <h2><div id="user"></div></h2>
            </font>
        	<!-- TL-Portal -->
            <p>Welcome to</p><br>
            <a href="https://sites.google.com/tlmshk.edu.hk/portal" border="0" alt="TL-Portal" target="portal">
                <img src="https://drive.google.com/uc?export=view&id=1lF8j8LglJ5RP6c1v0MYf5z1_lfT6hMHj" width="282" height="60">
            </a>
            <!--Add buttons to initiate auth sequence and sign out-->
            <p>Remember to</p><br>
            <button class="button buttonAlarm" id="signout-button" onClick="signOut()">Sign Out</button>
        </p>
	</div>
	
    <iframe id="signout" name="signout" style="display:none" onload="redirect()"></iframe>
    
    <script type="text/javascript">
		// Enter an API key from the Google API Console:
		//   https://console.developers.google.com/apis/credentials
		var apiKey = 'AIzaSyD2VwVfJ0BmkfQDp5QPQIgFf0fqhWusUuo';
		
		// Enter the API Discovery Docs that describes the APIs you want to
		// access. In this example, we are accessing the People API, so we load
		// Discovery Doc found here: https://developers.google.com/people/api/rest/
		var discoveryDocs = ["https://people.googleapis.com/$discovery/rest?version=v1"];
		
		// Enter a client ID for a web application from the Google API Console:
		//   https://console.developers.google.com/apis/credentials?project=_
		// In your API Console project, add a JavaScript origin that corresponds
		//   to the domain where you will be running the script.
		var clientId = '407887437328-cj7hp71din4gtpdr9tm7p5v50eveckg6.apps.googleusercontent.com';
		
		// Enter one or more authorization scopes. Refer to the documentation for
		// the API or https://developers.google.com/people/v1/how-tos/authorizing
		// for details.
		var scopes = 'profile';
		
		var signinButton = document.getElementById('signin-button');
		var signoutButton = document.getElementById('signout-button');
		
		function handleClientLoad() 
		{
			// Load the API client and auth2 library
			gapi.load('client:auth2', initClient);
		}
		
		function initClient() 
		{
			gapi.client.init(
			{
				apiKey: apiKey,
				discoveryDocs: discoveryDocs,
				clientId: clientId,
				scope: scopes
			}).then(function () 
			{
				// Listen for sign-in state changes.
				gapi.auth2.getAuthInstance().isSignedIn.listen(updateSigninStatus);
				
				// Handle the initial sign-in state.
				updateSigninStatus(gapi.auth2.getAuthInstance().isSignedIn.get());
			});
		}
		
		function updateSigninStatus(isSignedIn) 
		{
			if (isSignedIn) 
			{
				makeApiCall();
				signin.style.display = 'none';
				signed.style.display = 'block';
				window.open("https://sites.google.com/tlmshk.edu.hk/portal", "_blank");
			} 
		}
		
		function signIn(event) 
		{
			gapi.auth2.getAuthInstance().signIn();
		}
		
		function signOut(event) 
		{	
			document.getElementById('signout').src = "https://www.google.com/accounts/Logout";			
		}

		// Load the API and make an API call.  Display the results on the screen.
		function makeApiCall() 
		{
			gapi.client.people.people.get(
			{
				'resourceName': 'people/me',
				'requestMask.includeField': 'person.names'
			}).then(function(resp) 
			{
				var p = document.createElement('p');
				var name = resp.result.names[0].givenName + " " + resp.result.names[0].familyName;
				p.appendChild(document.createTextNode('Hello, '+name+'!'));
				document.getElementById('user').appendChild(p);
			});
		}
		
		function redirect()
		{
			// TL-Portal Sign In Page
			window.location = 'http://localhost/portal';
		}
	</script>
    
    <script async defer src="https://apis.google.com/js/api.js" 
        onload="this.onload=function(){};handleClientLoad()" 
        onreadystatechange="if (this.readyState === 'complete') this.onload()">
    </script>
</body>
</html>
