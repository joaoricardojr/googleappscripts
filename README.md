# Google App Scripts
My google app scripts and all the thigs necessary to create automation with google docs that i've been playing with.

<p>Google App Script uses a variation of JS called GS,  there are some similarities regarding edentation and coding but a complete reference to GS can be found here</p>
  <br><br><br>
We'll first begin with a nice one. How to populate a google doc with form answers!
Let's begin
<details><summary>
  <h2>How to Populate a google doc in with Google Forms answer</h2></summary>

  <ol>
    <li>Create a doc that will be a template and change dynamic values with tags that will be replaced by the answers on google forms</li>
    <li>Create a form with the questions related to the doc tags</li>
    <li>Create the script on Google App Script to collet the form answers and create a new doc with the information populated</li>
  </ol>
  <h3>Creating the Google Doc :bookmark_tabs:</h3>
  For this step you should consider the usage of {{}} to set your tags aside from the normal text, an example could be:<br>
  Hi my name is {{first_name}}<br>
  In this case, {{first_name}} will be the tag and will be replaced by our google forms.
  <h3>Creating the Google Forms :bookmark_tabs:</h3>
  Now we will create a form, the name will have to be easy to understand and the form should also point the answers to a Sheet.<br>
  To enable the form to save the responses on a sheet just click on "Responses" and select "View in Sheet".<br>
  Now the answers will be saved on that file. Also grab this file URL since we will need it later<br>
  <h3>Creating the App Script</h3>
  Now to create the App Script you must go to **Extensions> AppScript**. This will open the code editor with a black page like this one:<br>
  <img src="https://user-images.githubusercontent.com/15799220/214115565-80605841-e231-429c-8d01-fb82ceddd6d1.png"></img><br>
  Now we must type-in our code. I'll leave an example here that includes Texts (Short Answers), Strings (Checkboxes), Images (File Upload), and an e-mail   field to add that e-mail as an editor for the file.<br>
  <br>
  <br>
  
```javascript
/** Create new file and edit based on google forms */
function autoFillGoogleDocFromForm(e) {
/** Set the strings that will be changed to the information on the form */
	var timestamp = e.values[0];
	var stringName = e.values[1];
	var stringName2 = e.values[2];
	var logoImage  = e.values[3];
	var listOfStrings = e.values[4];
	var stringName3 = e.values[5];
	var email = e.values[6];


/** Specify what file is the template and where the populated file should be copied at */
	var templateFile = DriveApp.getFileById("The ID of the file that is the original template");
	var templateRespondeFolder = DriveApp.getFolderById("The ID of the destination folder for the new copy");
/** Gets the ID of the file on Drive and create a so called 'blob' that will be used to import the image */
	var fileID = logoImage.match(/[\w\_\-]{25,}/).toString();
	var image   = DriveApp.getFileById(fileID).getBlob();
	

/** Creates a auto-naming variable that uses the merchantName var */
	var copy = templateFile.makeCopy(merchantName+ ' - Template', templateRespondeFolder);
	
/** Set the doc variable to get the created document's Id */
	var doc = DocumentApp.openById(copy.getId());

/** Get body of document and swipe the tags with the variables */
	var body = doc.getBody();



	body.replaceText("{{string_name}}", stringName);
	body.replaceText("{{string_name2}}", stringName2);
	body.replaceText("{{string_list}}", listOfStrings);
	body.replaceText("{{string_name3}}", stringName3);

/** insert the image on the first page of de doc, adjust the dimensions as you preffer. I'm wodking on specifying the location of the image on the doc. */
	body.insertImage(0,image).setWidth(140).setHeight(40);
	

/**Add an new editor then save & close */
	doc.addEditor(email);
	doc.saveAndClose();
}
```
<br>
<br>
Now, if your form is already created with the proper fields, then answering it should generate a new file with string1 on its name and with all the information replaced and an image to be placed as you like on the new document.

<h3> Important! ❗ </h3>
Please always click "run" after saving the code on AppScript. Google asks for permission on every access you do inside the macro so you might bump into some erros of permission if you haven't clicked in "run on the AppScript Editor.


</details>

