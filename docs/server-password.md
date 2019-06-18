# Password Scrambling Algorithm

The `pserver` authentication protocol, as described in [Connection and Authentication](server-conn-auth.md), trivially encodes the passwords. This is only to prevent inadvertent compromise; it provides no protection against even a relatively unsophisticated attacker. For comparison, HTTP Basic Authentication (as described in RFC2068) uses BASE64 for a similar purpose. CVS uses its own algorithm, described here.

The scrambled password starts with `A`, which serves to identify the scrambling algorithm in use. After that follows a single octet for each character in the password, according to a fixed encoding. The values are shown here, with the encoded values in decimal. Control characters, space, and characters outside the invariant ISO 646 character set are not shown; such characters are not recommended for use in passwords. There is a long discussion of character set issues in [Protocol Notes](server-protocol-notes.md).

IN |  |  |  | OUT |  |  |
:---:|:--:|:--:|:--:|:-----:|:--:|:--:|
**DEC** | **HEX** | **CHAR** | → | **DEC** | **HEX** | **CHAR**
33 | 21 | ! | → | 120 | 78 | x
34 | 22 | " | → | 53 | 35 | 5
37 | 25 | % | → | 109 | 6D | m
38 | 26 | & | → | 72 | 48 | H
39 | 27 | ' | → | 108 | 6C | l
40 | 28 | ( | → | 70 | 46 | F
41 | 29 | ) | → | 64 | 40 | @
42 | 2A | * | → | 76 | 4C | L
43 | 2B | + | → | 67 | 43 | C
44 | 2C | , | → | 116 | 74 | t
45 | 2D | - | → | 74 | 4A | J
46 | 2E | . | → | 68 | 44 | D
47 | 2F | / | → | 87 | 57 | W
48 | 30 | 0 | → | 111 | 6F | o
49 | 31 | 1 | → | 52 | 34 | 4
50 | 32 | 2 | → | 75 | 4B | K
51 | 33 | 3 | → | 119 | 77 | w
52 | 34 | 4 | → | 49 | 31 | 1
53 | 35 | 5 | → | 34 | 22 | "
54 | 36 | 6 | → | 82 | 52 | R
55 | 37 | 7 | → | 81 | 51 | Q
56 | 38 | 8 | → | 95 | 5F | _
57 | 39 | 9 | → | 65 | 41 | A
58 | 3A | : | → | 112 | 70 | p
59 | 3B | ; | → | 86 | 56 | V
60 | 3C | < | → | 118 | 76 | v
61 | 3D | = | → | 110 | 6E | n
62 | 3E | > | → | 122 | 7A | z
63 | 3F | ? | → | 105 | 69 | i
65 | 41 | A | → | 57 | 39 | 9
66 | 42 | B | → | 83 | 53 | S
67 | 43 | C | → | 43 | 2B | +
68 | 44 | D | → | 46 | 2E | .
69 | 45 | E | → | 102 | 66 | f
70 | 46 | F | → | 40 | 28 | (
71 | 47 | G | → | 89 | 59 | Y
72 | 48 | H | → | 38 | 26 | &
73 | 49 | I | → | 103 | 67 | g
74 | 4A | J | → | 45 | 2D | -
75 | 4B | K | → | 50 | 32 | 2
76 | 4C | L | → | 42 | 2A | *
77 | 4D | M | → | 123 | 7B | {
78 | 4E | N | → | 91 | 5B | [
79 | 4F | O | → | 35 | 23 | #
80 | 50 | P | → | 125 | 7D | }
81 | 51 | Q | → | 55 | 37 | 7
82 | 52 | R | → | 54 | 36 | 6
83 | 53 | S | → | 66 | 42 | B
84 | 54 | T | → | 124 | 7C | |
85 | 55 | U | → | 126 | 7E | ~
86 | 56 | V | → | 59 | 3B | ;
87 | 57 | W | → | 47 | 2F | /
88 | 58 | X | → | 92 | 5C | \
89 | 59 | Y | → | 71 | 47 | G
90 | 5A | Z | → | 115 | 73 | s
95 | 5F | _ | → | 56 | 38 | 8
97 | 61 | a | → | 121 | 79 | y
98 | 62 | b | → | 117 | 75 | u
99 | 63 | c | → | 104 | 68 | h
100 | 64 | d | → | 101 | 65 | e
101 | 65 | e | → | 100 | 64 | d
102 | 66 | f | → | 69 | 45 | E
103 | 67 | g | → | 73 | 49 | I
104 | 68 | h | → | 99 | 63 | c
105 | 69 | i | → | 63 | 3F | ?
106 | 6A | j | → | 94 | 5E | ^
107 | 6B | k | → | 93 | 5D | ]
108 | 6C | l | → | 39 | 27 | '
109 | 6D | m | → | 37 | 25 | %
110 | 6E | n | → | 61 | 3D | =
111 | 6F | o | → | 48 | 30 | 0
112 | 70 | p | → | 58 | 3A | :
113 | 71 | q | → | 113 | 71 | q
114 | 72 | r | → | 32 | 20
115 | 73 | s | → | 90 | 5A | Z
116 | 74 | t | → | 44 | 2C | ,
117 | 75 | u | → | 98 | 62 | b
118 | 76 | v | → | 60 | 3C | <
119 | 77 | w | → | 51 | 33 | 3
120 | 78 | x | → | 33 | 21 | !
121 | 79 | y | → | 97 | 61 | a
122 | 7A | z | → | 62 | 3E | >
