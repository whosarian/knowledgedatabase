# Regular Expressions

!!! tip "Important tokens"

  - `.`: Joker: Any symbol
  - `^`: Beginning of the line
  - `$`: Ending of the line
  - `\d`: Digit: Any number (0-9)
  - `\w`: Word: Letters, numbers or underscores
  - `*`: The preceding symbol can occur from 0 times to infinitely often 
  - `+`: The preceding symbol has to occur at least once

## Character classes

!!! tip "If you're not looking for just anything, but want to narrow down a selection, you use square brackets `[]`."

  - `[aeiou]`: Any vocal
  - `[a-z]`: Lowercase letter from a to z
  - `[^0-9]`: Everything but a number