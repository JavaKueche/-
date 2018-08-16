### [��ޤͤȤ�����](https://twitter.com/yamanetoshi)

- [������뤳��](https://github.com/JavaKueche/great-okinawa/issues/26)

### ����

[Go ����ǤĤ��륤�󥿥ץ꥿](https://www.oreilly.co.jp/books/9784873118222/index.html)���Υѡ������򿿻��� scheme �Υѡ�������äƤߤ褦�ȻפäƤޤ���

### �Ȥ꤫����

�Ȥꤢ�������ȡ������������Ʋ��ϤǤ���褦�ˤʤ�ȴ򤷤���

### �׷�

�ʲ����ѡ����Ǥ������ɤ��Τ��ɤ�����

```
5
'xxx
(define x 5)
(define x (lambda (x) (+ 1 x)))
(define add4 (let ((x 4)) (lambda (y) (+ x y))))
'''

### �ȡ�����

�ʲ�������ʤΤ��ʡ�

- ILLEGAL
- EOF
- IDENT
- SYMBOL
- INT
- PLUS
- MINUS
- BANG
- ASTERISK
- SLASH
- LT
- GT
- LPAREN
- RPAREN
- DEFINE
- LET
- LAMBDA
- QUOTE

- ���ȡ�keywords �����

### �������

TestNextToken ��������ʲ�?

```
        input := `5
'xxx
(define x 5)
(define x (lambda (x) (+ 1 x)))
(define add4 (let ((x 4)) (lambda (y) (+ x y))))
`

        tests := []struct {
	        expectedType    token.TokenType
		expectedLiteral string
	}{
	        {token.INT, "5"},
		{token.SYMBOL, "xxx"},
		{token.LPAREN, "("},
		{token.DEFINE, "DEFINE"},
		{token.IDENT, "x"},
		{token INT, "5"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.DEFINE, "DEFINE"},
		{token.IDENT, "x"},
		{token.LPAREN, "("},
		{token.LAMBDA, "LAMBDA"},
		{token.LPAREN, "("},
		{token.IDENT, "x"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.IDENT, "+"},
		{token.INT, "1"},
		{token.IDENT, "x"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.DEFINE, "DEFINE"},
		{token.IDENT, "add4"},
		{token.LPAREN, "("},
		{token.LET, "let"},
		{token.LPAREN, "("},
		{token.LPAREN, "("},
		{token.IDENT, "x"},
		{token.INT "4"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.LAMBDA, "LAMBDA"},
		{token.LPAREN, "("},
		{token.IDENT, "y"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.IDENT, "+"},
		{token.IDENT, "x"},
		{token.IDENT, "y"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
	}

	l := New(input)
```

�ʲ�ά��

### ������Ϥμ���

- ���� lexer.go ��Ƨ��
- Lexer �Ϥ��Τޤ�
- New �⤽�Τޤ�
- NextToken �Ǳ�������Τϰʲ�?
 - +
 - -
 - !
 - *
 - /
 - <
 - >
 - (
 - )
 - define
 - let
 - lambda
 - '

���ȡ��ѥ����³�����ʲ����ʡ�

- skipWhitespace
- readChar
- newToken
- isLetter
- isDigit

���Ȥϰʲ���Ƥ�ʤΤ��ɤ���

- readIdentifier
- LookupIdent
- isDigit
- readNumber

### �Ƥ��Ȥ�

�ǽ�ξ��֤��ä� commit ����ޤ���

### �������

�ޤǤλ���褦�䤯�ѥ������Τǰ��٤��ξ��֤� commit ����ޤ�������³���ѡ����μ����ȥƥ��Ȥ�񤭤ޤ���

### ��ݥ��ȥ�

�ʲ��Ǥ���

- https://github.com/yamanetoshi/scheme_interpreter