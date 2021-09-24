<html>
<head>
    <title>TL-Portal</title>
    <meta charset='utf-8' />

	<style>    
		button, input[type=button], input[type=submit], input[type=reset]
		{
			font-family: "Trebuchet MS", "Arial Narrow", Helvetica, sans-serif;
			font-size: 1em;
			width: 100px;
			background-color: #eebb00; /* orange */
			color: white;
			border: none;
			border-radius: 15%;
			padding: 3px 3px;
			margin: 3px 3px;
			cursor: pointer;
			text-decoration: none;
			display: inline-block;
			-webkit-transition-duration: 0.3s; /* Safari */
			transition-duration: 0.3s;
		}
		button:hover, input[type=button]:hover, input[type=submit]:hover, input[type=reset]:hover
		{
			background-color: #ccff66;
			color: #4caf50; /* green */
			box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
		}
	</style>

</head>
<body>
	<div id="header" style="visibility:hidden; height:25px; margin-top:0px; margin-left:0px">
        <table width="100%" border="0">
            <tr>
                <td width="44%" align="right">
                	<font face="Arial" size="2">
                        <div id="user"></div>
					</font>
                </td>
                <td width="12%" align="center">
                	<a href="https://sites.google.com/tlmshk.edu.hk/portal" border="0" alt="TL-Portal" target="portal">
                    	<img src="https://drive.google.com/uc?export=view&id=11eLQdbP-AUW8jHxRF9qgTrSU2gTJeX22" width="140" height="30">
					</a>
				</td>
                <td width="44%">
                    <button id="signout-button" onClick="signOut()">Sign Out</button>
                </td>
            </tr>
        </table>
	</div>
	
    <div id="signin" style="display:block; height:auto; margin-top:25px; margin-left:0px">
        <p align="center">
        	<!-- School Badge -->
            <img src="https://drive.google.com/uc?export=view&id=1pwPSIdBLY4oNaFOsfVAF6kgMH8C-1zOs" height="35%">
            <br>
        	<!-- TL-Portal -->
        	<img src="https://drive.google.com/uc?export=view&id=1lF8j8LglJ5RP6c1v0MYf5z1_lfT6hMHj" height="12%">
            <br><br>
            <!--Add buttons to initiate auth sequence and sign out-->
            <button id="signin-button" onClick="signIn()">Sign In</button>
        </p>
	</div>
    
    <div id="content" style="display:none; height:auto; margin-top:25px; margin-left:0px">
<!--	
        <iframe 
        	id="portal" 
            name="portal" 
            title="TL-Portal" 
            style="border:0px; width:100%; height:100%; margin-top:0px; margin-left:0px">
        </iframe>
-->    
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
				showPortal();
			} 
		}
		
		function signIn(event) 
		{
			gapi.auth2.getAuthInstance().signIn();
		}
		
		function signOut(event) 
		{	
//			gapi.auth2.getAuthInstance().signOut();
/*
			var auth2 = gapi.auth2.getAuthInstance();
			auth2.signOut().then(function ()
			{
				auth2.disconnect();
			});
*/

//			document.location.href = "https://www.google.com/accounts/Logout?continue=https://appengine.google.com/_ah/logout?continue=https://www.google.com/accounts/optintoaccountchooser?optout=1";
			document.getElementById('signout').src = "https://www.google.com/accounts/Logout";			
/*
			header.style.visibility = 'hidden';
			login.style.display = 'block';
			content.style.display = 'none';	
*/
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
		
		function showPortal() 
		{
			var portal = document.createElement('iframe');
			portal.setAttribute('id', 'portal');
			portal.setAttribute('name', 'portal');
			portal.setAttribute('src', 'https://sites.google.com/tlmshk.edu.hk/portal');
			portal.style.marginTop = "25px";
			portal.style.marginLeft = "0px";
			portal.style.border = "0px";
			portal.style.width = "100%";
			portal.style.height = "100%";
			document.getElementById('content').appendChild(portal);

			header.style.visibility = 'visible';
			content.style.display = 'block';
			signin.style.display = 'none';

/*
			var p = document.getElementById('portal');
			p.src = "https://sites.google.com/tlmshk.edu.hk/portal";
			p.contentWindow.location.reload();
			content.style.display = 'block';
			header.style.visibility = 'visible';
			signin.style.display = 'none';
*/
		}
		
		function redirect()
		{
//			window.location = 'https://www.tlmshk.edu.hk';
			window.location = 'http://localhost/portal';
		}
	</script>
    
    <script async defer src="https://apis.google.com/js/api.js" 
        onload="this.onload=function(){};handleClientLoad()" 
        onreadystatechange="if (this.readyState === 'complete') this.onload()">
    </script>
</body>
</html>

