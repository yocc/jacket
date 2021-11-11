https://pkg.go.dev/regexp



### Overview [¶](https://pkg.go.dev/regexp#pkg-overview)

Package regexp implements regular expression search.

The syntax of the regular expressions accepted is the same general syntax used by Perl, Python, and other languages. More precisely, it is the syntax accepted by RE2 and described at https://golang.org/s/re2syntax, except for \C. For an overview of the syntax, run

```
go doc regexp/syntax
```

The regexp implementation provided by this package is guaranteed to run in time linear in the size of the input. (This is a property not guaranteed by most open source implementations of regular expressions.) For more information about this property, see

```
https://swtch.com/~rsc/regexp/regexp1.html
```

or any book about automata theory.

All characters are UTF-8-encoded code points.

There are 16 methods of Regexp that match a regular expression and identify the matched text. Their names are matched by this regular expression:

```
Find(All)?(String)?(Submatch)?(Index)?
```

If 'All' is present, the routine matches successive non-overlapping matches of the entire expression. Empty matches abutting a preceding match are ignored. The return value is a slice containing the successive return values of the corresponding non-'All' routine. These routines take an extra integer argument, n. If n >= 0, the function returns at most n matches/submatches; otherwise, it returns all of them.

If 'String' is present, the argument is a string; otherwise it is a slice of bytes; return values are adjusted as appropriate.

If 'Submatch' is present, the return value is a slice identifying the successive submatches of the expression. Submatches are matches of parenthesized subexpressions (also known as capturing groups) within the regular expression, numbered from left to right in order of opening parenthesis. Submatch 0 is the match of the entire expression, submatch 1 the match of the first parenthesized subexpression, and so on.

If 'Index' is present, matches and submatches are identified by byte index pairs within the input string: result[2*n:2*n+1] identifies the indexes of the nth submatch. The pair for n==0 identifies the match of the entire expression. If 'Index' is not present, the match is identified by the text of the match/submatch. If an index is negative or text is nil, it means that subexpression did not match any string in the input. For 'String' versions an empty string means either no match or an empty match.

There is also a subset of the methods that can be applied to text read from a RuneReader:

```
MatchReader, FindReaderIndex, FindReaderSubmatchIndex
```

This set may grow. Note that regular expression matches may need to examine text beyond the text returned by a match, so the methods that match text from a RuneReader may read arbitrarily far into the input before returning.

(There are a few other methods that do not match this pattern.)

<details tabindex="-1" id="example-package" class="Documentation-exampleDetails js-exampleContainer" style="box-sizing: border-box; border: 0px; font-style: inherit; font-variant: inherit; font-weight: inherit; font-stretch: inherit; line-height: inherit; font-family: inherit; font-size: 16px; margin: 1rem 0px 0px; padding: 0px; vertical-align: baseline; display: block;"><summary class="Documentation-exampleDetailsHeader" style="box-sizing: border-box; border: 0px; font-style: inherit; font-variant: inherit; font-weight: inherit; font-stretch: inherit; line-height: inherit; font-family: inherit; font-size: 16px; margin: 0px 0px 2rem; padding: 0px; vertical-align: baseline; color: var(--color-brand-primary); cursor: pointer; outline: none; text-decoration: none;">Example<span>&nbsp;</span><a href="https://pkg.go.dev/regexp#example-package" style="box-sizing: border-box; border: 0px; font-style: inherit; font-variant: inherit; font-weight: inherit; font-stretch: inherit; line-height: inherit; font-family: inherit; font-size: 16px; margin: 0px; padding: 0px; vertical-align: baseline; color: var(--color-brand-primary); text-decoration: none; opacity: 0;">¶</a></summary></details>

### Index [¶](https://pkg.go.dev/regexp#pkg-index)

- [func Match(pattern string, b [\]byte) (matched bool, err error)](https://pkg.go.dev/regexp#Match)
- [func MatchReader(pattern string, r io.RuneReader) (matched bool, err error)](https://pkg.go.dev/regexp#MatchReader)
- [func MatchString(pattern string, s string) (matched bool, err error)](https://pkg.go.dev/regexp#MatchString)
- [func QuoteMeta(s string) string](https://pkg.go.dev/regexp#QuoteMeta)
- [type Regexp](https://pkg.go.dev/regexp#Regexp)
- - [func Compile(expr string) (*Regexp, error)](https://pkg.go.dev/regexp#Compile)
  - [func CompilePOSIX(expr string) (*Regexp, error)](https://pkg.go.dev/regexp#CompilePOSIX)
  - [func MustCompile(str string) *Regexp](https://pkg.go.dev/regexp#MustCompile)
  - [func MustCompilePOSIX(str string) *Regexp](https://pkg.go.dev/regexp#MustCompilePOSIX)
- - [func (re *Regexp) Copy() *Regexp](https://pkg.go.dev/regexp#Regexp.Copy)DEPRECATED
  - [func (re *Regexp) Expand(dst [\]byte, template []byte, src []byte, match []int) []byte](https://pkg.go.dev/regexp#Regexp.Expand)
  - [func (re *Regexp) ExpandString(dst [\]byte, template string, src string, match []int) []byte](https://pkg.go.dev/regexp#Regexp.ExpandString)
  - [func (re *Regexp) Find(b [\]byte) []byte](https://pkg.go.dev/regexp#Regexp.Find)
  - [func (re *Regexp) FindAll(b [\]byte, n int) [][]byte](https://pkg.go.dev/regexp#Regexp.FindAll)
  - [func (re *Regexp) FindAllIndex(b [\]byte, n int) [][]int](https://pkg.go.dev/regexp#Regexp.FindAllIndex)
  - [func (re *Regexp) FindAllString(s string, n int) [\]string](https://pkg.go.dev/regexp#Regexp.FindAllString)
  - [func (re *Regexp) FindAllStringIndex(s string, n int) [\][]int](https://pkg.go.dev/regexp#Regexp.FindAllStringIndex)
  - [func (re *Regexp) FindAllStringSubmatch(s string, n int) [\][]string](https://pkg.go.dev/regexp#Regexp.FindAllStringSubmatch)
  - [func (re *Regexp) FindAllStringSubmatchIndex(s string, n int) [\][]int](https://pkg.go.dev/regexp#Regexp.FindAllStringSubmatchIndex)
  - [func (re *Regexp) FindAllSubmatch(b [\]byte, n int) [][][]byte](https://pkg.go.dev/regexp#Regexp.FindAllSubmatch)
  - [func (re *Regexp) FindAllSubmatchIndex(b [\]byte, n int) [][]int](https://pkg.go.dev/regexp#Regexp.FindAllSubmatchIndex)
  - [func (re *Regexp) FindIndex(b [\]byte) (loc []int)](https://pkg.go.dev/regexp#Regexp.FindIndex)
  - [func (re *Regexp) FindReaderIndex(r io.RuneReader) (loc [\]int)](https://pkg.go.dev/regexp#Regexp.FindReaderIndex)
  - [func (re *Regexp) FindReaderSubmatchIndex(r io.RuneReader) [\]int](https://pkg.go.dev/regexp#Regexp.FindReaderSubmatchIndex)
  - [func (re *Regexp) FindString(s string) string](https://pkg.go.dev/regexp#Regexp.FindString)
  - [func (re *Regexp) FindStringIndex(s string) (loc [\]int)](https://pkg.go.dev/regexp#Regexp.FindStringIndex)
  - [func (re *Regexp) FindStringSubmatch(s string) [\]string](https://pkg.go.dev/regexp#Regexp.FindStringSubmatch)
  - [func (re *Regexp) FindStringSubmatchIndex(s string) [\]int](https://pkg.go.dev/regexp#Regexp.FindStringSubmatchIndex)
  - [func (re *Regexp) FindSubmatch(b [\]byte) [][]byte](https://pkg.go.dev/regexp#Regexp.FindSubmatch)
  - [func (re *Regexp) FindSubmatchIndex(b [\]byte) []int](https://pkg.go.dev/regexp#Regexp.FindSubmatchIndex)
  - [func (re *Regexp) LiteralPrefix() (prefix string, complete bool)](https://pkg.go.dev/regexp#Regexp.LiteralPrefix)
  - [func (re *Regexp) Longest()](https://pkg.go.dev/regexp#Regexp.Longest)
  - [func (re *Regexp) Match(b [\]byte) bool](https://pkg.go.dev/regexp#Regexp.Match)
  - [func (re *Regexp) MatchReader(r io.RuneReader) bool](https://pkg.go.dev/regexp#Regexp.MatchReader)
  - [func (re *Regexp) MatchString(s string) bool](https://pkg.go.dev/regexp#Regexp.MatchString)
  - [func (re *Regexp) NumSubexp() int](https://pkg.go.dev/regexp#Regexp.NumSubexp)
  - [func (re *Regexp) ReplaceAll(src, repl [\]byte) []byte](https://pkg.go.dev/regexp#Regexp.ReplaceAll)
  - [func (re *Regexp) ReplaceAllFunc(src [\]byte, repl func([]byte) []byte) []byte](https://pkg.go.dev/regexp#Regexp.ReplaceAllFunc)
  - [func (re *Regexp) ReplaceAllLiteral(src, repl [\]byte) []byte](https://pkg.go.dev/regexp#Regexp.ReplaceAllLiteral)
  - [func (re *Regexp) ReplaceAllLiteralString(src, repl string) string](https://pkg.go.dev/regexp#Regexp.ReplaceAllLiteralString)
  - [func (re *Regexp) ReplaceAllString(src, repl string) string](https://pkg.go.dev/regexp#Regexp.ReplaceAllString)
  - [func (re *Regexp) ReplaceAllStringFunc(src string, repl func(string) string) string](https://pkg.go.dev/regexp#Regexp.ReplaceAllStringFunc)
  - [func (re *Regexp) Split(s string, n int) [\]string](https://pkg.go.dev/regexp#Regexp.Split)
  - [func (re *Regexp) String() string](https://pkg.go.dev/regexp#Regexp.String)
  - [func (re *Regexp) SubexpIndex(name string) int](https://pkg.go.dev/regexp#Regexp.SubexpIndex)
  - [func (re *Regexp) SubexpNames() [\]string](https://pkg.go.dev/regexp#Regexp.SubexpNames)

#### Examples [¶](https://pkg.go.dev/regexp#pkg-examples)

- [Package](https://pkg.go.dev/regexp#example-package)
- [Match](https://pkg.go.dev/regexp#example-Match)
- [MatchString](https://pkg.go.dev/regexp#example-MatchString)
- [QuoteMeta](https://pkg.go.dev/regexp#example-QuoteMeta)
- [Regexp.Expand](https://pkg.go.dev/regexp#example-Regexp.Expand)
- [Regexp.ExpandString](https://pkg.go.dev/regexp#example-Regexp.ExpandString)
- [Regexp.Find](https://pkg.go.dev/regexp#example-Regexp.Find)
- [Regexp.FindAll](https://pkg.go.dev/regexp#example-Regexp.FindAll)
- [Regexp.FindAllIndex](https://pkg.go.dev/regexp#example-Regexp.FindAllIndex)
- [Regexp.FindAllString](https://pkg.go.dev/regexp#example-Regexp.FindAllString)
- [Regexp.FindAllStringSubmatch](https://pkg.go.dev/regexp#example-Regexp.FindAllStringSubmatch)
- [Regexp.FindAllStringSubmatchIndex](https://pkg.go.dev/regexp#example-Regexp.FindAllStringSubmatchIndex)
- [Regexp.FindAllSubmatch](https://pkg.go.dev/regexp#example-Regexp.FindAllSubmatch)
- [Regexp.FindAllSubmatchIndex](https://pkg.go.dev/regexp#example-Regexp.FindAllSubmatchIndex)
- [Regexp.FindIndex](https://pkg.go.dev/regexp#example-Regexp.FindIndex)
- [Regexp.FindString](https://pkg.go.dev/regexp#example-Regexp.FindString)
- [Regexp.FindStringIndex](https://pkg.go.dev/regexp#example-Regexp.FindStringIndex)
- [Regexp.FindStringSubmatch](https://pkg.go.dev/regexp#example-Regexp.FindStringSubmatch)
- [Regexp.FindSubmatch](https://pkg.go.dev/regexp#example-Regexp.FindSubmatch)
- [Regexp.FindSubmatchIndex](https://pkg.go.dev/regexp#example-Regexp.FindSubmatchIndex)
- [Regexp.Longest](https://pkg.go.dev/regexp#example-Regexp.Longest)
- [Regexp.Match](https://pkg.go.dev/regexp#example-Regexp.Match)
- [Regexp.MatchString](https://pkg.go.dev/regexp#example-Regexp.MatchString)
- [Regexp.NumSubexp](https://pkg.go.dev/regexp#example-Regexp.NumSubexp)
- [Regexp.ReplaceAll](https://pkg.go.dev/regexp#example-Regexp.ReplaceAll)
- [Regexp.ReplaceAllLiteralString](https://pkg.go.dev/regexp#example-Regexp.ReplaceAllLiteralString)
- [Regexp.ReplaceAllString](https://pkg.go.dev/regexp#example-Regexp.ReplaceAllString)
- [Regexp.ReplaceAllStringFunc](https://pkg.go.dev/regexp#example-Regexp.ReplaceAllStringFunc)
- [Regexp.Split](https://pkg.go.dev/regexp#example-Regexp.Split)
- [Regexp.SubexpIndex](https://pkg.go.dev/regexp#example-Regexp.SubexpIndex)
- [Regexp.SubexpNames](https://pkg.go.dev/regexp#example-Regexp.SubexpNames)

### Constants [¶](https://pkg.go.dev/regexp#pkg-constants)

This section is empty.

### Variables [¶](https://pkg.go.dev/regexp#pkg-variables)

This section is empty.