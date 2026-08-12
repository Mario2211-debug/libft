*This project has been created as part of the 42 curriculum by mafonso.*

# libft

## Description

**libft** is my own implementation of a set of standard C library functions, packaged as a static library called `libft.a`.

It is the first project of the 42 cursus, and its purpose is practical rather than academic: later projects are not allowed to use most of the standard library, so every student builds a personal toolbox first and carries it forward. Every function here was written from scratch using only `malloc`, `free` and `write` — no calls to the original libc versions.

The library contains **34 functions**, split into two groups:

- **23 reimplementations of existing libc functions** (`strlen`, `memcpy`, `atoi`, …), which must behave exactly like the originals, including their return values and edge cases.
- **11 additional functions** that do not exist in libc but are useful in practice (`ft_split`, `ft_itoa`, `ft_strtrim`, …).

This repository contains the **mandatory part only**. The bonus part of the subject — the linked-list functions (`ft_lstnew`, `ft_lstadd_front`, and so on) — was not implemented, so there is no `t_list` type in the header.

---

## Building

Run `make` in the project root:

```bash
make
```

This produces `libft.a` in the current directory. Object files are written to an `obj/` folder so they do not clutter the source tree.

Other targets:

| Command | What it does |
| --- | --- |
| `make` / `make all` | Compiles the library into `libft.a` |
| `make clean` | Deletes the `obj/` folder with the object files |
| `make fclean` | Runs `clean` and also deletes `libft.a` |
| `make re` | Runs `fclean` and rebuilds everything |

Everything is compiled with `cc -Wall -Wextra -Werror`, as required by the subject. The build must produce zero warnings.

---

## Using it in your own program

Include the header and link the archive:

```c
#include "libft.h"

int main(void)
{
    char **words = ft_split("one two three", ' ');

    ft_putendl_fd(words[0], 1);   /* prints: one */
    return (0);
}
```

```bash
cc main.c -L. -lft -I. -o my_program
```

Or link the archive directly, which is what most 42 projects do:

```bash
cc main.c libft.a -o my_program
```

---

## Function reference

### Character checks and conversion

| Function | Description |
| --- | --- |
| `int ft_isalpha(int c)` | Returns non-zero if `c` is a letter. |
| `int ft_isdigit(int c)` | Returns non-zero if `c` is a digit from `0` to `9`. |
| `int ft_isalnum(int c)` | Returns non-zero if `c` is a letter or a digit. |
| `int ft_isascii(int c)` | Returns non-zero if `c` is a valid ASCII value (0–127). |
| `int ft_isprint(int c)` | Returns non-zero if `c` is a printable character, space included. |
| `int ft_toupper(int c)` | Converts a lowercase letter to uppercase; anything else is returned unchanged. |
| `int ft_tolower(int c)` | Converts an uppercase letter to lowercase; anything else is returned unchanged. |

### Memory

| Function | Description |
| --- | --- |
| `void *ft_memset(void *ptr, int c, size_t n)` | Fills the first `n` bytes of `ptr` with the byte `c`. Returns `ptr`. |
| `void ft_bzero(void *s, size_t n)` | Sets the first `n` bytes of `s` to zero. Returns nothing. |
| `void *ft_memcpy(void *dest, const void *src, size_t n)` | Copies `n` bytes from `src` to `dest`. The two areas must not overlap. |
| `void *ft_memmove(void *dest, const void *src, size_t n)` | Same as `ft_memcpy`, but safe when the areas overlap. |
| `void *ft_memchr(const void *str, int c, size_t n)` | Finds the first occurrence of byte `c` in the first `n` bytes. Returns a pointer to it, or `NULL`. |
| `int ft_memcmp(const void *s1, const void *s2, size_t n)` | Compares the first `n` bytes of two memory areas. |
| `void *ft_calloc(size_t nmemb, size_t size)` | Allocates memory for `nmemb` elements of `size` bytes and zeroes it. |

### Strings

| Function | Description |
| --- | --- |
| `size_t ft_strlen(const char *c)` | Returns the number of characters before the terminating `\0`. |
| `size_t ft_strlcpy(char *dest, const char *src, size_t n)` | Copies a string with size limit, always null-terminating. Returns the length of `src`. |
| `size_t ft_strlcat(char *dst, char const *src, size_t dstsize)` | Appends a string with size limit. Returns the length it tried to create. |
| `char *ft_strchr(const char *s, int c)` | Returns a pointer to the first occurrence of `c` in `s`, or `NULL`. |
| `char *ft_strrchr(const char *s, int c)` | Returns a pointer to the last occurrence of `c` in `s`, or `NULL`. |
| `int ft_strncmp(const char *s1, const char *s2, size_t n)` | Compares at most the first `n` characters of two strings. |
| `char *ft_strnstr(const char *big, const char *little, size_t len)` | Finds the substring `little` inside the first `len` characters of `big`. |
| `char *ft_strdup(const char *s)` | Returns a newly allocated copy of `s`. |
| `int ft_atoi(const char *str)` | Converts the leading numeric part of a string into an `int`. |

### Extra functions (not in libc)

Every function in this group returns freshly allocated memory unless stated otherwise, and returns `NULL` if the allocation fails. **The caller is responsible for freeing the result.**

| Function | Description |
| --- | --- |
| `char *ft_substr(char const *s, unsigned int start, size_t len)` | Returns a new string with at most `len` characters taken from `s`, starting at index `start`. |
| `char *ft_strjoin(char const *s1, char const *s2)` | Returns a new string made of `s1` followed by `s2`. |
| `char *ft_strtrim(char const *s1, const char *set)` | Returns a copy of `s1` with any leading and trailing characters that appear in `set` removed. |
| `char **ft_split(char const *s, char n)` | Splits `s` on the delimiter `n` and returns a `NULL`-terminated array of strings. Note the second parameter is a single delimiter character, not a string. |
| `char *ft_itoa(int n)` | Converts an `int` into a newly allocated string, negative numbers included. |
| `char *ft_strmapi(char const *s, char (*f)(unsigned int, char))` | Builds a new string by applying `f` to each character of `s` along with its index. |
| `void ft_striteri(char *s, void (*f)(unsigned int, char*))` | Applies `f` to each character of `s` in place, passing its index. Modifies `s` directly. |

### Writing to a file descriptor

These four write directly to a file descriptor using `write`. Pass `1` for standard output and `2` for standard error.

| Function | Description |
| --- | --- |
| `void ft_putchar_fd(char c, int fd)` | Writes a single character. |
| `void ft_putstr_fd(char *str, int fd)` | Writes a string, with no trailing newline. |
| `void ft_putendl_fd(char *s, int fd)` | Writes a string followed by a newline. |
| `void ft_putnbr_fd(int nb, int fd)` | Writes an integer in decimal. |

---

## Notes on the implementation

- Each function lives in its own `.c` file named after it, which is a hard requirement of the subject and also makes the archive easy to navigate.
- All prototypes are declared in `libft.h`, the only header you need to include.
- No global variables are used anywhere in the library.
- Functions that allocate memory always check the result of `malloc` before writing to it, and return `NULL` on failure instead of crashing.

---

## Resources

- `man 3 string`, `man 3 memory`, `man 2 write`
- The 42 libft subject PDF
- *The C Programming Language* — Kernighan & Ritchie
- The GNU C Library documentation, used to confirm the exact return values of the original functions

### AI usage

AI tools were used as a study aid: to clarify the documented behaviour of some libc functions in edge cases, and to review code for readability. The implementation, the testing and the debugging were done by me.
