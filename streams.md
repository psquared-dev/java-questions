##

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

All buffering, filtering, or conversion is done by specific subclasses, not by InputStream itself.

Core abstract method:

```java
public abstract int read() throws IOException;
```

This is the only method subclasses must implement.

### What InputStream actually provides

InputStream only provides:

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

These extend InputStream directly:

1. ByteArrayInputStream - Reads bytes from an in-memory byte array.
2. FileInputStream - Reads bytes from a file on disk (OS file descriptor).
3. FilterInputStream - Base class for filter streams (buffered, data, etc.).
4. ObjectInputStream - Reads Java objects (deserializes).
5. PipedInputStream - Used for piped inter-thread communication.
6. SequenceInputStream - Reads sequentially from multiple streams.
7. StringBufferInputStream (deprecated) - Reads bytes from a String (bad API due to charset issues).
8. SocketInputStream - The stream used internally by `java.net.Socket` to handle input from a network connection.

These are the only direct subclasses.

## Direct Subclasses of OutputStream

These extend OutputStream directly:

These extend OutputStream directly:

1. ByteArrayOutputStream - Writes bytes to a resizable in-memory buffer.
2. FileOutputStream - Writes bytes to a file on disk.
3. FilterOutputStream - Base class for filter output streams.
4. ObjectOutputStream - Writes Java objects (serialization).
5. PipedOutputStream - Pairs with PipedInputStream.

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

## Examples

### Reading the file using FileInputStream

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

* `System.out` is a `PrintStream`.
* Before writing anything, `PrintStream` must:

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
