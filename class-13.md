# **local Storage for Web Applications**
## HTML5 Strorge
is a way for web pages to store named key/value pairs locally, within the client web browser

![localstore](https://ui.vision/Content/Images/kantulocalstorage.jpg)

## USING HTML5 STORAGE
  * HTML5 Storage is based on named key/value pairs. 
  * You store data based on a named key, then you can retrieve that data with the same key. 
  * The named key is a string. 
  * The data can be any type supported by JavaScript, including strings, Booleans, integers, or floats.
  *  However, the data is actually stored as a string
  * If you are storing and retrieving anything other than strings, you will need to use functions like **parseInt()** or **parseFloat()** to coerce your retrieved data into the expected JavaScript datatype.
  * Calling setItem() with a named key that already exists will silently overwrite the previous value. Calling getItem() with a non-existent key will return null rather than throw an exception.
  * Local storge has four basic methods 
  ![localmethod](https://res.cloudinary.com/practicaldev/image/fetch/s--rSskJpsi--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://dev-to-uploads.s3.amazonaws.com/i/2imjutnczd4f3jdhgbdx.png) 

  ###  StorageEvent Object
   A StorageEvent is sent to a window when a storage area it has access to is changed within the context of another document.

   * #### StorageEvent Object properties
   ![stobp](https://www.programmersought.com/images/35/8fc50290594cf12a3769afd0b4b40cd3.png)


   


