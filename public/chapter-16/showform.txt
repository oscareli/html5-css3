<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
    <title>Processing Form Data</title>
	<style type="text/css">
	/* The CSS is here to keep the demo simple. As always, I recommend you put your CSS in an external file. */
	body {
		font-size: 100%;
		font-family: Arial, sans-serif;
	}
	</style>
</head>
<body>
<p>This is a very simple PHP script that outputs the name of each bit of information (that corresponds to the <code>name</code> attribute for that form field) along with the value that was sent with it right in the browser window.</p>

<p>In a more useful script, you might store this information in a MySQL database, or send it to your email address.</p>

<p><strong>Note: The sample PHP code in this file is simple by design, but as a result, <em>it doesn't include the security checks that a bulletproof script would include</em>.  Use caution before including it on your own site. If you do intend to use a script like this, I recommend consulting PHP books and other resources to learn how to check submitted form values for malicious data before you write the values to the screen or to a database.</strong></p>

<!-- Form Data  -->
<table>
<tr>
	<th>Field Name</th>
	<th>Value(s)</th>
</tr>

<?php
if (empty($_POST)) {
	print "<tr><td colspan=\"2\"><p>No data was submitted.</p></td></tr>";
} else { /* Data was submitted, so show it in the page. */
	foreach ($_POST as $key => $value) {
		
		/* Cleans up quoted values. See: 
				http://us2.php.net/manual/en/function.get-magic-quotes-gpc.php
				http://us2.php.net/manual/en/function.stripslashes.php
		*/
		if (get_magic_quotes_gpc()) {
			$value = stripslashes($value);
		}
		
		/* Check the form field and print its value in a table row */
		if ($key == 'email_signup') { // True if one of the Email checkboxes at end
			
			if (is_array($_POST['email_signup'])) { // True if a checkbox checked
				// Print the name of the checkbox form field and the value
				foreach ($_POST['email_signup'] as $value) {
					print "<tr><td><code>$value</code></td><td>"; 
					print "<i>on</i>";
					print "</td></tr>\n";
				}
			} else {
				print "<tr><td><code>$key</code></td><td><i>$value</i></td></tr>\n";
			}
			
		} else { // Print row and info for form fields except the Email checkboxes
			print "<tr><td><code>$key</code></td><td><i>$value</i></td></tr>\n";
		}
	}
}

/* Display the file upload picture name */
if(isset($_FILES['picture'])) {
	/*
		This basic script presents the name of the uploaded file only. To check the file size, file type (like JPG, GIF, or PNG), and actually upload a file to a folder, see the video tutorial at http://net.tutsplus.com/articles/news/diving-into-php/. The code explained in the video is also available for download from that URL. That page also includes links to a series of videos about using PHP.
	*/

	$picture_name = $_FILES['picture']['name'];
	print "<tr><td><code>picture</code></td><td><i>$picture_name</i></td></tr>\n";
}

?>
</table>
</body>
</html>
