# Information about this entry

Morse codes for letters and digits use sequences of 1 to 5 (inclusive)
Morse symbols (denoted as `.` or `-`), e.g.

```
    E .
    Z --..
    3 ...--
```

(BTW: digits always have 5 symbols, letters 1 to 4.  I didn't use this fact.)

Of course we can encode these symbols with bits in a byte.  I chose `-`
to be a bit set to 0, `.` to be a bit set to 1.  Characters in C are at
least 8 bits, but they are either signed or unsigned.  To be portable,
I've used only values from 0 to 127 (inclusive) (both in the encoding
of Morse codes, as well as in subexpressions in the program).  Hence we
can use type `char` (using `unsigned char` for an extra bit would have made
the source code excessively large ;-)

The least significant bit contains the first symbol of a Morse code, so
we have

```
    E > 0......1
    Z > 0...1100
    3 > 0..00111
```

The dots still have to be determined.  But we also need to encode the
length of the sequence.  Since there are only 4 different kind of lengths,
two bits would suffice, e.g.

```
    E > 000....1
    Z > 010.1100
    3 > 01100111
```

That means bits 6 and 5 set to 00 is length 1, set to 01 is length 2, etc.
But this would mean that we would have to extract the length into a
counter before shifting out each symbol, such as

``` <!---c-->
    length = (encoding & 96) >> 5;
    do {
            putchar(encoding & 1 ? '-' : '.');
            encoding >>= 1;
    } while (length--);
```

We can also use the remaining bits to *include* the counter

```
    length 1: 00000.1x     `E > 00000.11
    length 2: 0000.1xx
    length 3: 000.1xxx
    length 4: 00.1xxxx     Z > 00.11100
    length 5: 0.1xxxxx     3 > 0.100111
```

with 1 bit still to be chosen freely (let's assume it is 0).  Now we can do

``` <!---c-->
    do {
            putchar(encoding & 1 ? '-' : '.');
            encoding >>= 1;
    while (encoding > 1);
```

As you see, the encoded Morse sequence can be the counter itself. But
this is not very pleasant, as some Morse sequences will end up having
an ASCII value < 32, i.e. they will be control codes.  This would make
the initializing (encoding) string awkward.  Hence I came up with the
following

```
    length 1: 0010010x     E > 00100101  (= '%')
    length 2: 001010xx
    length 3: 00110xxx
    length 4: 0100xxxx     Z > 01001100  (= 'L')
    length 5: 011xxxxx     3 > 01100111  (= 'g')
```

Notice these encodings in the initialization string.  Only the coding for
`5` needs an escape (`5 = ..... > 01111111 = '\177'`).  The way the length
is encoded still allows the encoding to be its own counter:

``` <!---c-->
    do {
            putchar(encoding & 1 ? '-' : '.');
            encoding = (encoding + 32) >> 1;
    } while (encoding > 35);
```

The last shift will always result in `00100010`, hence the test for `>35`.
Note that the calculation for the next value of `encoding` is done at `int`
precision, so that the actual value of the `char` remains `<= 127`.

The character `'-'` (ASCII 45) and `'.'` (ASCII 46) are conveniently in
sequence, so the `putchar(3)` can simply be

``` <!---c-->
    putchar(45 + encoding % 2);
```

I used the modulo operator this time, because `+` takes precedence over `&`.
The assignment can be incorporated into the `while` control test, and a
`do-while` loop with a body of a single expression statement can be written
as a `while` loop with a comma operator in the test:

``` <!---c-->
    while(putchar(45 + encoding % 2),
        encoding = encoding + 32 >> 1);
```

Voila.

Now, the decoding of Morse codes is just the reverse.  We shift in `0`s
and `1`s as we scan the line.  But: we have to scan from back to front
(the left most Morse symbol is in the least significant bit, and thus
has to shifted in last).  The test for the scanning while loop must
stop when there are no more `.` or `-` on the left.  Hence we need a
terminating character in front of the line.  The input line is read one
past the encoding string (into the same character array) the terminating
character for the reverse scan is therefore provided by the implicit `'\0'`
of the array initializer ! The test also stops when at most 5 symbols
have been shifted in.

The encoded character is then looked up in the encoding string with
`memchr(3)` (why `memchr(3)` is explained later).  Either a `?` (when there are
more than 5 consecutive Morse symbols, or if the sequence is unknown)
or the decoded character is printed.  Or a `' '` if there was an extra
space in the source line.  Note the use of "or" in these last sentences,
as they explain the use of the conditional operator.

The `strspn(3)` calls need either `" .-"` or `".-"` as argument.  Of course they
are overlapped.  I put these strings between the encodings for digits
and letters.  This is OK, since neither of these characters, or the `'\0'`,
is a valid encoding !  We must, however, use `memchr(3)` instead of `strchr(3)` in
the decoding lookup because of this `'\0'`.  Since it is far more obscure
this way, I hope you'll forgive me.

The outline of the program is roughly as follows (pseudo C):

```
    while (more lines) {
        if (line is in morse) {
            while (more characters) {
                skip next sequence
                    if (' ')
                        putchar(' ');
                    else
                        decode_sequence; /* backwards */
            }
        } else {
            while (more characters) {
                if (isalnum)
                    encode_character;
                    putchar(' ');
            }
        }
```

Because of the *two* `while(more characters)` loops are the same, I've
exchanged the `if(line is in morse)` and the two `while` loops (so there
is *single* `while` loop).  However, we must now store the `if(line is in
morse)` test for efficiency (it would be stupid to test the entire line
for every character).  This is done in `l[0]` !  (the first encoding is
thus in `l[1]`).  The `memchr(3)` decoding lookup is not disturbed, because the
test is always `0` for Morse lines, and `'\0'` is not a possible encoding.
`l[0]` is also used as the temporary shift character while encoding a
line of text.  This does not disturb the `if (line is in morse)` test,
as the temporary `l[0]` will remain `!= 0`.

I've put the `if`s in the above layout into a single switch.  Hope you don't
mind ;-)

Summary of the use of `l[999]`:

- `l[0]`
  * **result of "is this line in morse" test**

- `l[0]`
  * **temporary shift during encoding**

- `l[0]  - l[33]`
  * **Morse codings (letters and digits, plus garbage)**

- `l[11] - l[14]`
  * **`strspn(3)` argument**

- `l[12] - l[14]`
  * **`strspn(3)` argument**

- `l[34]`
  * **terminator for backward decoding**

- `l[35] - l[998]`
  * **input line**

<hr style="width:10%;text-align:left;margin-left:0">

Selected notes:

- We don't need to declare `isalnum(3)` (or include `ctype.h`) as its implicit
  declaration is correct (`int` argument, `int` result).  This is not so for
  `strlen(3)`, `strspn(3)`, and `memchr(3)` as they use `size_t`.
- During development, my gcc 2.7.2.3 had an internal compiler error (signal
  6\) on code that was correct !
- lclint 2.4b thinks that "the observer is modified" a couple of times,
  whereas it is not.  Well, it is, but there is a sequence point in between.
- lclint 2.4b parses [line 13](%%REPO_URL%%/1998/dorssel/dorssel.c#L13) as `<error>`, whereas it is correct code.
- lclint 2.4b is positive the second `while` loop is infinite, whereas it is
  not.

<hr style="width:10%;text-align:left;margin-left:0">

..  - .... .. -. -.-  - .... .. ...  ... .--. --- .. .-.. . .-.  .. ...  -- --- .-. .  --- -... ..-. ..- ... -.-. .- - . -..  - .... .- -.  - .... .  .--. .-. --- --. .-. .- --

. -..- . .-. -.-. .. ... .
-.-. .... .- -. --. .  - .... .  .--. .-. --- --. .-. .- --  ... ---  .. -  -.. --- . ...  -. --- -  ... .... --- ..- -


<hr style="width:10%;text-align:left;margin-left:0">

Jump to: [top](#)


<!--

    Copyright © 1984-2024 by Landon Curt Noll. All Rights Reserved.

    You are free to share and adapt this file under the terms of this license:

        Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)

    For more information, see:

        https://creativecommons.org/licenses/by-sa/4.0/

-->
