## Client Code
```html
<form action="#" method="GET">
			Username:<br />
			<input type="text" name="username"><br />
			Password:<br />
			<input type="password" AUTOCOMPLETE="off" name="password"><br />
			<br />
			<input type="submit" value="Login" name="Login">

		</form>
		<pre><br />Username and/or password incorrect.</pre>

```

![[Pasted image 20240630195353.png]]
## Server Code

```php
<?php

if( isset( $_GET[ 'Login' ] ) ) {
    // Get username
    $user = $_GET[ 'username' ];

    // Get password
    $pass = $_GET[ 'password' ];
    $pass = md5( $pass );

    // Check the database
    $query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    if( $result && mysqli_num_rows( $result ) == 1 ) {
        // Get users details
        $row    = mysqli_fetch_assoc( $result );
        $avatar = $row["avatar"];

        // Login successful
        echo "<p>Welcome to the password protected area {$user}</p>";
        echo "<img src=\"{$avatar}\" />";
    }
    else {
        // Login failed
        echo "<pre><br />Username and/or password incorrect.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?> 
```

## Attack
We can see the server code dosen't check how many request we send. 

If we try to insert incorrect user and password we see the site return "**Username and/or password incorrect.**"
![[Pasted image 20240630223532.png]]

So i see url request with BurpSuite and i take it.
![[Pasted image 20240630205122.png]]

I create this python script to bruteforce the login page and check if in the response return the **Username and/or password incorrect** phrase or not.

## Results
The credential founded to login are:

| Username | Password |
| -------- | -------- |
| admin    | password |
| Admin    | password |
![[Credentials.png]]
