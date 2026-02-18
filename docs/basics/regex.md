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

## Examples

E-Mail: `[\w.-]+@[\w.-]+\.[a-z]{2,3}`

1. `[\w.-]+`: One ore more letters, dots or hyphens
2. `@`
3. `[\w.-]+`: same as 1
4. `\.`: Dot
5. `[a-z]{2,3}`: 2 or 3 lowercase letters (.com)

Dates: `\d{2}\.\d{2}\.\d{4}`

1. `\d{2}`: Day (two digits)
2. `\.`: Dot
3. `\d{2}\: Month (two digits)
4. `\.`: Dot
5. `\d{4}`: Year (four digits)
