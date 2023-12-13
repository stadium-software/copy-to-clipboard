# Copy To Clipboard

Highlighting text to copy to the clipboard can be cumbersome and annoying. Here's a script to allow users to copy all the content or just the selected text of a *TextBox* or a *Label* to the clipboard by clicking a link.

# Version 
1.0 Initial

# Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

# Copy Control Setup

## Copy Control Global Script Setup
1. Create a Global Script called "CopyToClipboard"
2. Add the input parameter below to the Global Script
   1. ClassName
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/*Stadium Script Version 1.0*/
let className = ~.Parameters.Input.ClassName;
let el = document.querySelectorAll("." + className);
if (el.length == 0 || el.length > 1) {
    console.error("The class '" + className + "' must be assigned to exactly one control");
    return false;
} else { 
    el = el[0];
}

let copyDivToClipboard = () => {
    let range = document.createRange();
    range.selectNode(el);
    window.getSelection().removeAllRanges();
    window.getSelection().addRange(range);
    document.execCommand("copy");
    window.getSelection().removeAllRanges();
};
copyDivToClipboard();
```

## Page Setup
1. Drag a *TextBox* or a *Label* control to a page
2. Add a classname to uniquely identify the control (e.g. copy-to-clipboard)
3. Enter some text in the control *Text* property
4. Drag a *Link*, *Button* or *Image* control to the same page
5. Create a *Click Event Handler* for the *Link*, *Button* or *Image* control
6. Drag the script called "CopyToClipboard" into the Event Handler
7. Enter the script input parameter
   1. ClassName: The class you assigned to the control to be copied (e.g. copy-to-clipboard)
8. Optional: Show a *Notification* to indicate to the user that the copy has been successful

# Copy Selected Text Setup

## Copy Control Global Script Setup
1. Create a Global Script called "CopyToSelectedClipboard"
2. Drag a *JavaScript* action into the script
3. Add the Javascript below into the JavaScript code property
```javascript
/*Stadium Script Version 1.0*/
let copyDivToClipboard = (el) => {
    let range = document.createRange();
    range.selectNode(el);
    window.getSelection().removeAllRanges();
    window.getSelection().addRange(range);
    document.execCommand("copy");
    window.getSelection().removeAllRanges();
};
let getSelectedText = () => {
    var txt = "";
    if (window.getSelection) {
        txt = window.getSelection();
    } else if (document.getSelection) {
        txt = document.getSelection();
    } else if (document.selection) {
        txt = document.selection.createRange().text;
    } else return;
    let tmpEl = document.createElement("span");
    tmpEl.innerText = txt;
    document.body.appendChild(tmpEl);
    copyDivToClipboard(tmpEl);
    tmpEl.remove();
};
getSelectedText();
```

## Page Setup
1. Drag a *TextBox* or a *Label* control to a page
2. Enter some text in the control *Text* property
3. Drag a *Link*, *Button* or *Image* control to the same page
4. Create a *Click Event Handler* for the *Link*, *Button* or *Image* control
5. Drag the script called "CopySelectedToClipboard" into the Event Handler
6. Optional: Show a *Notification* to indicate to the user that the copy has been successful