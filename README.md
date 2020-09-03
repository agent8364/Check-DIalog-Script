# Check-DIalog-Script

///Checks if a string is short enough to fit a window; if not, splits it into multiple strings.
///Returns strings as list
///@arg string
///@arg Width
var str = argument[0]; //String to split
var len = argument[1]; //Max width in pixels
var out = ds_list_create();

//Split the string into words
var wordList;
wordList[0] = "";
var w = 0;


for (var i = 1; i <= string_length(str); i++) { //For each character in string
    var char = string_char_at(str, i);
    if (char == " ") {
        w++;
        wordList[w] = "";
    }
    else
        wordList[w] += char;
}

var w = 1;
var split = wordList[0];
do {
    //Check if the current word is on our word replacement list
    if (wordList[w] == "[nl]") { //New Line shortcut
        ds_list_add(out, split);
        split = "";
        w++;
    } else if (string_width(split + " " + wordList[w]) < len) {
        if (split != "")
            split += " ";
        split += wordList[w];
        w++;

    } else {
        ds_list_add(out, split);
        split = wordList[w];
        w++;
    }
} until (w == array_length_1d(wordList));
//Add any remaining words to the array
if (split != "")
    ds_list_add(out, split);

//Return the final array
return out;
