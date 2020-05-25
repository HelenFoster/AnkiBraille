### Front

```HTML
<p class="meaning">{{Meaning}}</p>

{{type:Braille}}

<script>
//Braille dot entry with ES7 + jQuery
//Copyright Helen Foster, 2020
//License: MIT; https://opensource.org/licenses/MIT

//You can edit the key bindings here.
//Braille dots 123456 with keys FDSJKL:
var dotKeys = [70,68,83,74,75,76];
//delete last with "h" or semicolon (real backspace key still works anyway):
var backspaceKeys = [72,186];
//finish with enter or "a":
var doneKeys = [13,65];
//block normal action of a-y to prevent accidental text entry
//(will be done anyway for keys listed above; keeping z for undo):
function keyPrevented(k) {
    return k >= 65 && k <= 89;
}

//map of dot numbers to Braille Ascii
var dotMap = {
    "1": "a", "12": "b", "14": "c", "145": "d", "15": "e",
    "124": "f", "1245": "g", "125": "h", "24": "i", "245": "j",
    "13": "k", "123": "l", "134": "m", "1345": "n", "135": "o",
    "1234": "p", "12345": "q", "1235": "r", "234": "s", "2345": "t",
    "136": "u", "1236": "v", "1346": "x", "13456": "y", "1356": "z",
    "12346": "&", "123456": "=", "12356": "(", "2346": "!", "23456": ")",
    "16": "*", "126": "<", "146": "%", "1456": "?", "156": ":",
    "1246": "$", "12456": "]", "1256": "\\", "246": "[", "2456": "w",
    "2": "1", "23": "2", "25": "3", "256": "4", "26": "5",
    "235": "6", "2356": "7", "236": "8", "35": "9", "356": "0",
    "34": "/", "346": "+", "3456": "#", "345": ">", "3": "'", "36": "-",
    "4": "@", "45": "^", "456": "_", "5": "\"", "46": ".", "56": ";", "6": ","
};

function makeDotEntry(textInput, doneCallback) {
    var dots = new Set();

    textInput.keydown(function(event) {
        var k = event.which;
        if (dotKeys.includes(k)) {
            event.preventDefault();
            dots.add(dotKeys.indexOf(k) + 1);
        } else if (backspaceKeys.includes(k)) {
            event.preventDefault();
            textInput.val(textInput.val().slice(0, -1));
        } else if (doneKeys.includes(k)) {
            event.preventDefault();
            doneCallback();
        } else if (keyPrevented(k)) {
            event.preventDefault();
        }
    });

    textInput.keyup(function(event) {
        if (dots.size > 0) {
            var dotsArr = Array.from(dots);
            dotsArr.sort();
            var dotsStr = dotsArr.join("");
            textInput.val(textInput.val() + dotMap[dotsStr]);
            dots.clear();
        }
    });
}
</script>

<script>
//Anki glue code
$(document).ready(function() {
    var typeans = $("#typeans");
    typeans.removeAttr("onkeypress");
    typeans.removeAttr("style");
    typeans.addClass("braille");
    makeDotEntry(typeans, function() {
        pycmd("ans");
    });
});
</script>
```

### Back


```HTML
{{FrontSide}}

<hr id=answer>

<p>{{Notes}}</p>

<p>{{Mnemonic}}</p>

```
