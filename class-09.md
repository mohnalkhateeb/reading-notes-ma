# **Forms and JS Events**
## HTML Form Structur 
 - The HTML < form > element is used to create an HTML form for user input
![formstruc](images/htmlform.png)
### Text Input
- The < input > tag specifies an input field where the user can enter data.

- The < input > element is the most important form element.

- The < input > element can be displayed in several ways, depending on the type attribute.

![txtinput](https://miro.medium.com/max/2078/1*5QuZu94szBQ3Pwh-9YGfUA.png)

### Password Input
 - < input type="password" >

![passwordin](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRSM-Lye9JXqf_TLb89gw9jaxZPLICuegjls_sSJkVqM4IaINgdDjxNhGA0rQGhKDiUCKM&usqp=CAU)

### Text Area 
 - The ***textarea*** element is used to create a mutli-line text input. Unlike other input elements this is not an empty element. It should therefore have an opening and a closing tag.

![txtarea](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRCFGxNPtkOUy-b-ZajuBOropqPcP9nE_Htque0p6bKovU9VQbsn2lxrH_SFO_ol7XnHnU&usqp=CAU)

### Radio Button
  - Radio buttons allow users to pick just one of a number of options.

![radiobutton](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRi98e-uKEBj6FModwOQgJGd4MyrO6JeMED7Q&usqp=CAU)

### Checkbox 
  - Checkboxes allow users to select (and unselect) one or more options in answer to a question.

 ![checkbox](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTVrWPu64g9K6GA6pBHmkNxbeVI1gm-IQqwjA&usqp=CAU)

### Drop Down List Input
 - A drop down list box (also known as a select box) allows users to select one option from a drop down list. The < select > element is used to create a drop down list box. It contains two or more < option > elements.

![dropin](https://i.stack.imgur.com/uRr21.png)

### Multiple select Box

![multiplesbox](https://cdn-anlbg.nitrocdn.com/dKKErbUyoNysjatCgltCzbTJJilTMwLi/assets/static/optimized/rev-4b21c3b/wp-content/uploads/sites/1/nggallery/selenium-1/5-HTML-Code-for-Multi-Select.png)

### File Input Box
 - If you want to allow users to upload a file (for example an image, video, mp3, or a PDF), you will need to use a file input box.

![filein](https://image.slidesharecdn.com/lesson7cssforms-181121231142/95/lesson-7-css-forms-29-638.jpg)

### Submit Button
 - The submit button is used to send a form to the server.

![subuttn](https://webmasternow.com/wp-content/uploads/2020/01/Submit-and-Reset.png)

### Image Button
 - f you want to use an image for the submit button, you can give the type attribute a value of image. The src, width, height, and alt attributes work just like they do when used with the < img > element 

### Labelling Form Controls
  - When introducing form controls, the code was kept simple by indicating the purpose of each one in text next to it. However, each form control should have its own <label> element as this makes the form accessible to vision-impaired users.
  -The ***for*** attribute states which form control the label belongs to. Note how the radio buttons use the id attribute. The value of the id attribute uniquely identifies an element from all other elements on a page.

 ![lableco](images/lablehtml.png)

### Grouping Form Elements
  - You can group related form controls together inside the < fieldset > element. This is particularly helpful for longer forms.
  - The < legend> element can come directly after the opening < fieldset> tag and contains a caption which helps identify the purpose of that group of form controls.

 ![GFE](https://image.slidesharecdn.com/webforms-160304143845/95/getting-information-through-html-forms-8-638.jpg?cb=1457112058)
### Form Validation

![validation](images/validation.jpg)

### Date Input 

![datein](https://i.imgur.com/Mf1LwYk.png)

### Email and URL Input

![emalurl](images/emailurl.png)

### HTML 5: Search Input
![serchin](https://lh3.googleusercontent.com/proxy/CeYyyeJ5lJuDgPkmC3EHMNSADsRcVdpM6ATltQ113vihdEMd-57rYA47TMChgd9ysnrUsKgg6GBHkqkHqsCBMqVYDBOWHQHEnAMljkkdGJJNDF21HsxN)

### bullet Point Styles (list-style-type)
 - The list-style-type property allows you to control the shape or style of a bullet point (also known as a marker). It can be used on rules that apply to the < ol >, < ul >, and < li > elements.

 ![liststyle](https://wpastra.com/wp-content/uploads/2017/11/bullet-lists-code.png)

### Images for Bullets (list-style-image)
 - You can specify an image to act as a bullet point using the list-style-image property.
 - The value starts with the letters url and is followed by a pair of parentheses. Inside the parentheses, the path to the image is given inside double quotes.
 - This property can be used on rules that apply to the < ul > and < li > elements.

 ![listimag](https://i.ytimg.com/vi/w7xKQ63DnDk/maxresdefault.jpg)

### positioning the Marker (list-style-position)
 - Lists are indented into the page by default and the list-styleposition property indicates whether the marker should appear on the inside or the outside of the box containing the main points.

 ![marklist](https://i1.wp.com/css-tricks.com/wp-content/uploads/2021/02/CleanShot-2021-02-15-at-07.00.59@2x.png?fit=1672%2C1162&ssl=1)

### List Shorthand (list-style)
 - As with several of the other CSS properties, there is a property that acts as a shorthand for list styles. It is called list-style, and it allows you to express the markers' style, image and position properties in any order.
 
### Table Properties

![tablepro](https://stuyhsdesign.files.wordpress.com/2016/02/properties.png)

### Border on Empty Cells 
 - If you have empty cells in your table, then you can use the empty-cells property to specify whether or not their borders should be shown.
 - Since browsers treat empty cells in different ways, if you want to explicitly show or hide borders on any empty cells then you should use this property.
 - values : show,hide ,intial and inherit 

### Gaps Between Cells (border-spacing, border-collapse)
 - border-spacing

 ![borderspace](https://images.comparisonnetwork123.com/img/kompyuteri/76/atribut-tablic-border-spacing-v-css_1.jpg)

 - border-collapse

 ![bordcoll](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRS8OM5kBEPGxoJR-knaiJSCzD8-8--6Kz2gA&usqp=CAU)

### Cursor Style
- The cursor property allows you to control the type of mouse cursor that should be displayed
to users.

![cursor](https://camo.githubusercontent.com/f34fa72a5d52f7a219721e97de2ab95e5d4a2b31d9993d895a3a28540e7606a3/687474703a2f2f7765732e696f2f647261472f636f6e74656e74)





