| **Pattern**  | **Explanation**                                              | **Example**                                                  |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| literals     | Matches the exact characters                                 | li matches flip lip plier                                    |
| [group]      | Matches a single character in a group                        | [at] matches cat, cat, top, pear                             |
| [^group]     | Matches a single character not in the group                  | [^at] matches **c**at, t**o**p, to**p**, **p**ear, p**e**ar, pea**r** |
| [first-last] | Matches any character in the range first to last             | [0-9] matches digits **1**02, 1**0**2, 10**2**               |
| {n}          | The element is matched exactly n times                       | **91{2}** matches **911**                                    |
| {n,}         | The element is matched n or more times                       | wel{1,} matches well and **wel**come                         |
| {n,m}        | The element is matched between n and m times                 | 9{2,4} matches 99, 999, 9999, 99999 but not 9                |
| .            | Wildcard, any character except n                             | a.e matches ate and are                                      |
| *            | The element is matched zero or more times                    | d*.d matches .1, 0.1, 10.1 but not 10                        |
| +            | The element is matched one or more times                     | d*.d matches 0.1, 10.1 but not 10 or .1                      |
| ?            | The element is matched zero or one time                      | tr?ap matches trap and tap                                   |
| \|           | Matches any one of the elements separated by the \|          | th(e\|is\|at) matches the, this, that                        |
| [[:class:]]  | Matches the character class                                  | [[:upper:]] matches uppercase characters: I am Richard       |
| n            | Matches a newline                                            |                                                              |
| s            | Matches any single whitespace                                |                                                              |
| d            | Matches any single digit                                     | d is [0-9]                                                   |
| w            | Matches a character that can be in a word (upper case and lower case characters) |                                                              |
| b            | Matches at a boundary between alphanumeric characters and non-alphanumeric characters | d{2}b matches 999 and 9999 bd{2} matches 999 and 9999        |
| $            | End of the line                                              | s$ matches a single white space at the end of a line         |
| ^            | Start of line                                                | ^d matches if a line starts with a digit                     |