If you have a button or another element that triggers a menu widget, the convention is to place keyboard focus on the first menu item. If the menu is a popup and is position relative to the body, the page will scroll to where the menu is added to the page instead of remaining where the popup menu is displayed, negatively affecting user experience.

The traditional way to solve this is to use the following CSS rules:

```CSS
body.popup-open {
    overflow: hidden; /* Hides the scrollbar */
    position: fixed;  /* Fixes the body to the viewport */
    width: 100%;      /* Maintains body width */
}
```
and JavaScript:

```JavaScript
document.body.classList.add('popup-open');
document.body.style.top = '-' + window.scrollY + 'px';
document.documentElement.style.overflow = 'hidden';
```
However, doing the above will cause the page content to jump considerably due to the scrollbar disappearing, which can be unsettling to users. To solve this and provide a smooth experience when focus is moved to the popup menu, I create an transparent popup containing a button that is focused before moving the focus to the popup menu that the user sees and then hide the transparent popup:

The transparent popup, which is a child of the body tag.

```HTML
<div id="transparent-popup"><button type="button" id="transparent-popup-btn">Focus me</button></div>
```
```CSS
#transparent-popup {
    overflow: hidden;
    opacity: 0;
    z-index: 100000;
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    display: none;
}
```
JavaScript:

```JavaScript
var popup = document.getElementById("transparent-popup");//Get the transparent popop
popup.style.display = "block" //Show invisible popup
document.getElementById("transparent-popup-btn").focus();//Please keyboard focus on the button in the popup.

//Schedule to move the keyboard focus to the first menuitem of the popup menu after 100 milliseconds to prevent the page from jumping.
setTimeout(function() {
  //Now, move the keyboard focus the first item of the menu that the user triggered.
  document.getElementsByClassName("menu-item")[0].focus();
  popup.style.display = "none" //Hide invisible popup
},100);
```
