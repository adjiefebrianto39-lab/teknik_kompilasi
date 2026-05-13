# 🧠 Mini Compiler – AST, Parser, dan Three Address Code (TAC)

## 📌 Deskripsi
Project ini merupakan implementasi sederhana Mini Compiler yang mencakup:
- Abstract Syntax Tree (AST)
- Parser ekspresi matematika
- Analisis semantik sederhana
- Generasi Three Address Code (TAC)

Operator yang didukung:
+ , - , * , / , ^ (pangkat)

---

## 🏗️ 1. Struktur AST

AST digunakan untuk merepresentasikan struktur ekspresi dalam bentuk pohon.

```python
class AST:
    pass

class BinOp(AST):
    def __init__(self, left, op, right):
        self.left = left
        self.op = op
        self.right = right

class Num(AST):
    def __init__(self, value):
        self.value = value

class Var(AST):
    def __init__(self, name):
        self.name = name

## 2. Implementasi Mini Compiler
🔹 Power Function (Operator ^)
def power(self):
    node = self.factor()

    while self._current == '^':
        op = self._current
        self.advance()
        node = BinOp(left=node, op=op, right=self.factor())

    return node
🔹 Term Function (* dan /)
def term(self):
    node = self.power()

    while self._current in ('*', '/'):
        op = self._current
        self.advance()
        node = BinOp(left=node, op=op, right=self.power())

    return node
🔹 Expression Function (+ dan -)
def expr(self):
    node = self.term()

    while self._current in ('+', '-'):
        op = self._current
        self.advance()
        node = BinOp(left=node, op=op, right=self.term())

    return node

## 3. Contoh Pengujian
Input
source_code = "a ^ 2 + b * c"
symbol_table = {'a': 5, 'b': 10, 'c': 2}
Output Three Address Code (TAC)
t1 = a ^ 2
t2 = b * c
t3 = t1 + t2

## 4. Jawaban Pertanyaan Refleksi
1. Mengapa power() dipanggil di dalam term()?

Karena aturan operator precedence:

^ (pangkat) memiliki prioritas lebih tinggi daripada * dan /

Struktur parsing harus:

expr → term → power → factor

Jika dibalik, hasil perhitungan bisa salah karena urutan operasi tidak sesuai aturan matematika.

2. Apa yang terjadi jika variabel tidak ada di symbol table?

Jika variabel tidak ditemukan, maka akan muncul:

Semantic Error: Undefined variable

Ini terjadi pada tahap analisis semantik, yaitu pengecekan validitas variabel sebelum eksekusi.

3. Mengapa a ^ 2 harus dieksekusi sebelum + dalam TAC?

Karena TAC harus mengikuti urutan operasi berdasarkan dependency:

Operasi harus diselesaikan terlebih dahulu sebelum digunakan
Hasil a ^ 2 diperlukan untuk operasi penjumlahan

Contoh urutan benar:

t1 = a ^ 2
t2 = b * c
t3 = t1 + t2

Jika tidak, compiler tidak dapat menentukan nilai operand dengan benar.
