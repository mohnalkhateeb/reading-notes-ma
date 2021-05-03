* # HTML5 Layout
  * ### Traditional HTML Layouts
    - web page authors used <div> elements to group together related elements
    - Authors used class or id attributes to indicate the role of the <div> element in the structure of the page.
    ![Trad HTML lay](https://csharpcorner.azureedge.net/UploadFile/b5be7f/working-with-new-semantic-elements-in-html5-along-with-html/Images/Html%20Basic%20Structure%20Image.png)
  * ### New Html5 Layout Elements
    - HTML5 introduces a new set of elements that allow you to divide up the parts of a page. The names of these elements indicate the kind of content you will find in them. They are still subject to change, but that has not stopped many web page authors using them already.
    - figre below show html5 layout elements
    ![HTML5 Layout](https://stuyhsdesign.files.wordpress.com/2016/05/yoko-html5.png)
    - The new HTML5 elements indicate the purpose of different parts of a web page and help to describe its structure.
    - The new elements provide clearer code (compared with using multiple <div> elements).
    - Older browsers that do not understand HTML5 elements need to be told which elements are block-level elements.
    - To make HTML5 elements work in Internet Explorer 8 (and older versions of IE), extra JavaScript is needed, which is available free from Google.

# **Layout**

* CSS treats each HTML element as if it is in its own box. This box will either be a block-level box or an inline box.

![inbloc](https://miro.medium.com/max/2800/1*AFeOAqXNJJdfYAjfXiJ9AQ.jpeg)

* If one block-level element sits inside another block-level element then the outer box is known as the containing or parent element.
 
* ### How does CSS controll in page layout using position?

    * 1- **Normal flow(static)** : Every block-level element appears on a new line, causing each item to appear lower down the page than the previous one.
    * 2- **Relative Positioning** : This moves an element from the position it would be in normal flow, shifting it to the top, right, bottom, or left of where it would have been placed.
    * 3- **Absolute positioning** : This positions the elementin relation to its containing element. It is taken out of normal flow. Absolutely positioned elements move as users scroll up and down the page.

    
    * 4- **Fixed Positioning** :  This is a form of absolute positioning that positions the element in relation to the browser window, as opposed to the containing element.

    ![cssposition](https://www.csssolid.com/images/csspositions/css-position-all.png)

* ### What is Box offset?
    -  it is properties to tell the browser how far from the top or bottom and left or right it should be placed. 
* ##### Types of box offset

    * 1- **Floating Elements** : Floating an element allows you to take that element out of normal flow and position it to the far left or right of a containing box.
        ![floatel](https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/03/web-text-wrap.png?resize=540%2C270&ssl=1.png)

    -  The ***clear*** property allows you to say that no element (within the same containing element) should touch the left or righthand sides of a box.
    ![clearcss](https://i2.wp.com/css-tricks.com/wp-content/uploads/2021/03/directionalclearing.png?resize=540%2C226&ssl=1) 

    * 2- **Overlaping Elements**
    When you use relative, fixed, or absolute positioning, boxes can overlap. If boxes do overlap, the elements that appear later in the HTML code sit on top of those that are earlier in the page. If you want to control which element sits on top, you can use the z-index property. Its value is a number, and the higher the number the closer that element is to the front.

    ![zindex](https://aws1.discourse-cdn.com/business5/uploads/webflow1/original/3X/0/4/04f72d3ae81d11ef08825209d716ee2445bb43e8.png)

* #### Screen Size

    - Different visitors to your site will have different sized screens that show different amounts of information, so your design needs to be able to work on a range of different sized screens.
* #### Screen Resolution
    - Resolution refers to the number of dots a screen shows per inch. Some devices have a higher resolution than desktop computers and most operating systems allow users to adjust the resolution of their screens.
* #### Page Size
    - Because screen sizes and display resolutions vary so much, web designers often try to create pages of around 960-1000 pixels wide (since most users will be able to see designs this wide on their screens).

* #### Fixed width Layout
    - Fixed width layout designs do not change size as the user increases or decreases the size of their browser window. Measurements tend to be given in pixels.

    ![fixlay](https://www.templatemonster.com/blog/wp-content/uploads/2017/05/fixed-width1.png)
* #### Liquid Layout
    - Liquid layout designs stretch and contract as the user increases or decreases the size of their browser window. They tend to use percentages.
    ![liqlay1](https://matthewjamestaylor.com/blog/perfect-3-column-dimensions.gif)
    ![liqlay2](https://hackernoon.com/drafts/g4on3z5f.png)
