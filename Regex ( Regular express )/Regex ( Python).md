
| Name           | Description                                                                                                                             | example                      |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| e(letter)      | search for that letter                                                                                                                  |                              |
| ea?(letter)    | search for first letter and optional for letter that near ?                                                                             |                              |
| he*            | search for first letter and select all the same letter of the second                                                                    | thee,                        |
| \w             | match any word                                                                                                                          |                              |
| \s             | match any space                                                                                                                         |                              |
| \S             | match anything, which not space                                                                                                         |                              |
| \W             | match anything, which not word                                                                                                          |                              |
| \w{4,5}        | select word that have min 4 and max 5                                                                                                   |                              |
| [fc]at         | select all the word start letter in square Bracket                                                                                      | fat, cat,                    |
| [a-z]at        | select all the word start letter in square Bracket with range from a-z                                                                  | fat, cat, eat                |
| [a-z-A-Z]at    | select all the word start letter in square Bracket with range from a-z. Also select capital letter and lower case. It also take number. | fat, cat, eat, Fat, Cat, Eat |
| (t\|T)he       | search for word with or operator                                                                                                        | the, The                     |
| (t\|h\|e){2,3} | select the keyword min at 2 and max 3 and count after reach max                                                                         | ==thehee==                   |
| e{2,3 }        |                                                                                                                                         |                              |
