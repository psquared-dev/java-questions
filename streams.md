

Let’s begin at the foundation: the base classes of Java’s byte-stream system.

* `InputStream` (base class for reading bytes)
* `OutputStream` (base class for writing bytes)

Understanding these two gives you full clarity for everything built on top.

## InputStream — Base Class for Reading Bytes

`java.io.InputStream` is an **abstract**, unbuffered, byte-oriented base class.

Key facts (correct):

* It defines the API for reading bytes.
* It does not buffer data.
* It does not transform or filter data.
* It does not decode characters.
* It does not provide any performance optimizations.

All buffering, filtering, or conversion is done by specific subclasses, not by `InputStream` itself.

Core abstract method:

```java
public abstract int read() throws IOException;
```

This is the only method subclasses must implement.

### What InputStream actually provides

`InputStream` only provides:

1. The contract for reading single or multiple bytes.
2. Default implementations of:

    ```java
    int read(byte[] b)
    int read(byte[] b, int off, int len)
    ```
    These default implementations simply call the single-byte `read()` repeatedly.

3. `skip()`, `available()`, `close()` — default or minimal behavior.
4. No buffering whatsoever.

Important:

If you create your own `InputStream` subclass, and its `read()` method reads from a slow source (e.g., disk or network), 
you will get one system call per byte unless you wrap it in a buffered stream.

## OutputStream — Base Class for Writing Bytes

`java.io.OutputStream` is the **abstract** base class for writing bytes.

Correct facts:

* No buffering.
* No character conversion.
* No performance optimizations.
* Only defines the API contract.

Core abstract method:

```java
public abstract void write(int b) throws IOException;
```

All other write methods are conveniences layered on top.


## Direct Subclasses of InputStream

These extend `InputStream` directly:

1. `ByteArrayInputStream` - Reads bytes from an in-memory byte array.
2. `FileInputStream` - Reads bytes from a file on disk (OS file descriptor).
3. `FilterInputStream` - Base class for filter streams (buffered, data, etc.).
4. `ObjectInputStream` - Reads Java objects (deserializes).
5. `PipedInputStream` - Used for piped inter-thread communication.
6. `SequenceInputStream` - Reads sequentially from multiple streams.
7. `StringBufferInputStream` (deprecated) - Reads bytes from a String (bad API due to charset issues).
8. `SocketInputStream` - The stream used internally by `java.net.Socket` to handle input from a network connection.

These are the only direct subclasses.

## Direct Subclasses of OutputStream

These extend OutputStream directly:

These extend OutputStream directly:

1. `ByteArrayOutputStream` - Writes bytes to a resizable in-memory buffer.
2. `FileOutputStream` - Writes bytes to a file on disk.
3. `FilterOutputStream` - Base class for filter output streams.
4. `ObjectOutputStream` - Writes Java objects (serialization).
5. `PipedOutputStream` - Pairs with `PipedInputStream`.

These are the only direct subclasses.

## String Representation

In memory, Java strings are stored in UTF-16 (2-byte code units). 

Before working with examples, it is important to understand how Java stores strings in memory and how character 
data ends up in files.

Consider the following code:

```java
public class Example01 {
    public static void main(String[] args) throws IOException {

        String s = "A ₹ 🔥";

        byte[] bytes = s.getBytes(StandardCharsets.UTF_16BE);
        for (byte b : bytes) {
                System.out.printf("%02X ", b);
        }

        FileWriter fileWriter = new FileWriter("file2.txt");

        for (char c : s.toCharArray()) {
            fileWriter.write(c);
        }

        fileWriter.close();
    }
}
```

**Output (UTF-16BE bytes of the Java String):**

```bash
00 41 00 20 20 B9 00 20 D8 3D DD 25 
```

These bytes represent the string in Java’s internal UTF-16 form:

* `A` → `00 41`
* space → `00 20`
* `₹` (U+20B9) → `20 B9`
* space → `00 20`
* `🔥` (U+1F525) → surrogate pair `D8 3D DD 25`

### What actually gets written to the file?

The `FileWriter` does not write UTF-16.

It converts each char to the platform’s default encoding (usually UTF-8) and writes those bytes.

Running hexdump on the file:

```bash
(base) $ hexdump -C file2.txt 
00000000  41 20 e2 82 b9 20 f0 9f  94 a5                    |A ... ....|
0000000a
```

Which is the UTF-8 encoding of:

* `A` → `41`
* space → `20`
* `₹` → `E2 82 B9`
* space → `20`
* `🔥` → `F0 9F 94 A5`

## Reading bytes using FileInputStream

Consider the following file.txt:

```text
this is a file with ₹
```

Bytes representation is as follows:

```bash
(base) $ hexdump -C file.txt 
00000000  74 68 69 73 20 69 73 20  61 20 66 69 6c 65 20 77  |this is a file w|
00000010  69 74 68 20 e2 82 b9                              |ith ...|
00000017
```


```java
public class Example01 {
    public static void main(String[] args) throws IOException {
        FileInputStream fio = new FileInputStream("file.txt");
        int ch;
        while ( (ch = fio.read()) != -1){
            System.out.print(Integer.toHexString(ch) + " ");
        }

        fio.close();
    }
}
```

**Output:**

```bash
74 68 69 73 20 69 73 20 61 20 66 69 6c 65 20 77 69 74 68 20 e2 82 b9
```

The important thing to note here what happens when we call `System.out.print(Integer.toHexString(ch) + " ")`.

Lets explore.

### What happens when you call - System.out.print()

Conside the following simplified example:

```java
System.out.print("ab");
```

#### Step 1 — "ab" is a Java String

Java stores it internally in UTF-16:

```text
'a' → U+0061
'b' → U+0062
```

#### Step 2 — PrintStream converts the String to bytes

`System.out` is a `PrintStream`.

Before writing anything, `PrintStream` must:

1. Take your Java String
2. Convert it into bytes, using the console's charset
   * Usually UTF-8 on Linux/Mac
   * Often Cp1252 or UTF-8 on Windows

For UTF-8 console, `"ab"` becomes:

```bash
61 62
```

2 bytes (ASCII characters have same value in UTF-8).

#### Step 3 — Those bytes are written to the terminal

`PrintStream` writes:

```text
61 62   (bytes)
```

to the terminal/console. The terminal receives the bytes and displays:

```bash
ab
```

#### Summary — in one line
Yes — `"ab"` is converted to bytes using the system's charset (usually UTF-8) and those bytes are 
sent to the terminal.


### Drawbacks of using FileInputStream

`FileInputStream` works at the byte level, not the character level.

This creates two major limitations when handling text files:

1. It reads one raw byte at a time (not characters)

`FileInputStream.read()` returns an int in the range 0–255.

So when you read text:

* ASCII letters work fine (1 byte each)
* UTF-8 characters that take 2, 3, or 4 bytes do NOT work correctly

Examples:

* `₹` = `E2 82 B9` 
* `🔥` = `F0 9F 94 A5`

`FileInputStream` gives you each byte separately:


```text
F0, 9F, 94, A5
```

If you cast each byte:

```text
(char)0xF0  (char)0x9F  (char)0x94  (char)0xA5
```

You get garbage, because you broke the UTF-8 sequence.

2. `FileInputStream` does NOT understand characters or encodings

It has no idea about:

* UTF-8
* UTF-16
* ISO-8859-1
* ASCII
* surrogate pairs
* multi-byte sequences
* line endings

It only returns bytes.

If you want to turn bytes into characters, you need `InputStreamReader`, so that Java can decode UTF-8 properly:

```java
InputStreamReader reader =
     new InputStreamReader(new FileInputStream("file.txt"), StandardCharsets.UTF_8);
```

This solves:

* multi-byte characters
* surrogate pairs
* character boundaries
* emoji
* accents
* currency symbols


## Reading bytes using BufferedInputStream

```java
public class Example02 {
    public static void main(String[] args) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.txt"));
        int ch;
        while ( (ch = bis.read()) != -1){
            System.out.print(Integer.toHexString(ch) + " ");
        }

        bis.close();
    }
}
```

**Output:**

```bash
74 68 69 73 20 69 73 20 61 20 66 69 6c 65 20 77 69 74 68 20 e2 82 b9 
```

What `BufferedInputStream` actually changes

`FileInputStream.read()`

* Reads 1 byte from the OS every time
* Every `read()` call may trigger a **system call** → slow

`BufferedInputStream.read()`

* Reads **a block of bytes at once** from the OS (default buffer = 8192 bytes)
* Stores them in an internal byte array
* Future `read()` calls simply take the next byte from that array
(no system call until the buffer is empty)


## Reading characters using InputStreamReader

```java
public class Example03 {
    public static void main(String[] args) throws IOException {
        InputStreamReader inputStreamReader =
                new InputStreamReader(new BufferedInputStream(new FileInputStream("file.txt")));
        int ch;
        while ( (ch = inputStreamReader.read()) != -1){
            System.out.print((char)ch);
        }

        inputStreamReader.close();
    }
}
```

**Output:**

```bash
this is a file with ₹
```

`InputStreamReader` converts bytes → chars

`InputStreamReader` uses platform default encoding, e.g., UTF-8.

It reads as many bytes as needed to decode exact one Java char.

| Character | UTF-8 bytes   | UTF-16 (Java char values)        |
|-----------|---------------|----------------------------------|
| A         | `41`          | `0x0041`                         |
| space     | `20`          | `0x0020`                         |
| ₹         | `E2 82 B9`    | `0x20B9`                         |
| 🔥        | `F0 9F 94 A5` | `0xD83D 0xDD25` (surrogate pair) |

It is important to note that the `InputStreamReader` uses default encoding, this only works correctly if
**file is encoded using platform’s default charset**

If you want correct parsing 100% of time, specify encoding:

```java
InputStreamReader inputStreamReader =
    new InputStreamReader(
        new BufferedInputStream(new FileInputStream("file.txt")),
        StandardCharsets.UTF_8
    );
```


## Reading characters using FileReader

```java
InputStreamReader inputStreamReader =
                new InputStreamReader(new FileInputStream("file.txt"), StandardCharsets.UTF_8);
```

This call can be shortened using `FileReader` as follows: 

```java
FileReader fileReader = new FileReader("file.txt", StandardCharsets.UTF_8);
```

```java
public class Example04 {
    public static void main(String[] args) throws IOException {
        FileReader fileReader = new FileReader("file.txt", StandardCharsets.UTF_8);

        int ch;
        while ((ch = fileReader.read()) != -1) {
            System.out.print((char) ch);
        }

        fileReader.close();
    }
}
```

**Output:**

```bash
this is a file with ₹
```

## Reading using BufferedReader

The above program can be optimized using `BufferedReader`. Here is an example: 

```java
public class Example04 {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new FileReader("file.txt", StandardCharsets.UTF_8));

        int ch;
        while ((ch = bufferedReader.read()) != -1) {
            System.out.print((char) ch);
        }

        bufferedReader.close();
    }
}
```

**Output:**

```bash
this is a file with ₹
```

`BufferedReader` gives convenience method like `readLine()` to allow reading line by line.


## Writing bytes using FileOutputStream

```java
public class Example05 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fileOutputStream = new FileOutputStream("out1.txt");
        String s = "this is a file with ₹";
        byte[] bytes = s.getBytes(StandardCharsets.UTF_8);

        for (byte aByte : bytes) {
            fileOutputStream.write(aByte);
        }

        fileOutputStream.close();
    }
}
```

This creates a file named `out1.txt` with the following content:

```bash
(base) $ cat out1.txt 
this is a file with ₹
```

`FileOutputStream.write()` calls one OS `write()` syscall per byte.

A syscall is expensive because:

* it switches from user mode → kernel mode
* data crosses JVM boundary
* kernel does file-system bookkeeping
* then it switches back to user mode

If your string has 20 bytes, you make 20 syscalls.

If you write a large file (say 5 MB), you make millions of syscalls → extremely slow.


## Writing bytes using BufferedOutputStream

```java
public class Example06 {
    public static void main(String[] args) throws IOException {
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("out1.txt"));
        String s = "this is a file with ₹";
        byte[] bytes = s.getBytes(StandardCharsets.UTF_8);

        for (byte aByte : bytes) {
            bos.write(aByte);
        }

        bos.close();
    }
}
```

What's happening:

* Every `write(b)` writes into the **8 KB memory buffer** of `BufferedOutputStream`.
* No disk I/O happens until the buffer fills or you close/flush.
* With 20–30 bytes, everything fits in the buffer → **no syscalls in the loop**.
* At `.close()` → `BufferedOutputStream` flushes contents in one syscall.

This is much faster than raw `FileOutputStream`.


## Writing using OutputStreamWriter

Previously, the code explicitly converted the String to UTF-8 bytes using `s.getBytes(StandardCharsets.UTF_8)`. 
When using `OutputStreamWriter`, this conversion is performed automatically, so you do not need to call
`getBytes()` yourself.

```java
public class Example06 {
    public static void main(String[] args) throws IOException {
        OutputStreamWriter writer =
                new OutputStreamWriter(new FileOutputStream("out1.txt"), StandardCharsets.UTF_8);
        String s = "this is a file with ₹";

        writer.write(s);
        writer.close();
    }
}
```

Here the `writer`:

* Receives characters
* Encodes them to UTF-8 bytes
* Writes the bytes to `FileOutputStream`


## Writing using BufferedWriter

Previously, we simplified the way bytes were generated, but this required removing buffering because `OutputStreamWriter`
isn’t compatible with `BufferedOutputStream`. To add buffering back, we can wrap the `OutputStreamWriter` in 
a `BufferedWriter`. Here is an example:

```java
public class Example06 {
    public static void main(String[] args) throws IOException {
        BufferedWriter writer =
                new BufferedWriter(new OutputStreamWriter(new FileOutputStream("out1.txt"), StandardCharsets.UTF_8));
        String s = "this is a file with ₹";

        writer.write(s);
        writer.close();
    }
}
```

## Writing using FileWriter

In previous examples, we created `OutputStreamWriter` as follows:

```java
new OutputStreamWriter(new FileOutputStream("out1.txt"), StandardCharsets.UTF_8);
```

This can be simplified using the `FileWriter`.

```java
new FileWriter("out1.txt", StandardCharsets.UTF_8);
```

Here is an example:

```java
public class Example06 {
    public static void main(String[] args) throws IOException {
        BufferedWriter writer = new BufferedWriter(new FileWriter("out1.txt", StandardCharsets.UTF_8));
        String s = "this is a file with ₹";

        writer.write(s);
        writer.close();
    }
}
```

## Writing using PrintWriter

`PrintWriter` is a high-level text-output convenience class that makes it easy to write:

It adds following capabilities on top of normal writers:

1. Convenience methods

    Instead of:
    
    ```java
    writer.write("Hello");
    writer.write("\n");
    ```
    
    You can do:
    
    ```java
    printWriter.println("Hello");
    ```

2. Automatic charset conversion

    It wraps any `Writer` or `OutputStream`.

3. Formatting strings - `printf()` / `format()`

    Like C’s `printf`:
    
    ```java
    pw.printf("Total: %d items, cost: ₹%.2f", count, amount);
    ```

4. Auto-flush on newline (optional)

    ```java
    PrintWriter pw = new PrintWriter(writer, true);
    ```

    Now whenever you call:
    
    ```java
    pw.println("Hello");
    ```

    it flushes automatically.

5. Graceful error handling

    `PrintWriter` never throws `IOException`.  Instead, you can check:
    
    ```java
    pw.checkError();
    ```
   

## Writing using PrintStream

Most Java I/O classes allow writing:

* int (as a byte)
* byte[]
* char
* char[]
* String (via writers)

`PrintStream` exists mainly as a convenience wrapper that automatically converts any printable data type 
(text, numbers, booleans, objects) into bytes and writes them to an `OutputStream`.

### 🔥 Think of it this way:

`OutputStream` Understands only bytes.

`PrintStream` Sits on top and gives you easy printing methods:

* `print(String)`
* `print(int)`
* `print(double)`
* `println()`
* `printf()`

And does the conversions internally.





