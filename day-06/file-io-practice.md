# Day 06 – Linux Fundamentals: Read and Write Text Files

## Objective

Today I practiced basic Linux file handling using `touch`, `echo`, `>`, `>>`, `tee`, `cat`, `head`, and `tail`.

The goal was to create a text file, write data into it, append new lines, and read the file in different ways.

---

## 1. Create a File

### Command

```bash
touch notes.txt
```

### Output

```text
```

No output is displayed when `touch` successfully creates the file.

---

## 2. Write Text Using `>`

### Command

```bash
echo "Linux file handling is important in DevOps." > notes.txt
```

### Output

```text
```

The command writes the text into `notes.txt`.

`>` is used to write or overwrite the file.

---

## 3. Append Text Using `>>`

### Command

```bash
echo "I am learning basic Linux commands." >> notes.txt
```

### Output

```text
```

`>>` adds the new text at the end of the file without removing the existing content.

---

## 4. Use `tee` to Write and Display

### Command

```bash
echo "Reading and writing files is useful for DevOps." | tee -a notes.txt
```

### Output

```text
Reading and writing files is useful for DevOps.
```

`tee` displays the text on the terminal and also writes it to the file.

The `-a` option means append.

---

## 5. Read the Complete File Using `cat`

### Command

```bash
cat notes.txt
```

### Output

```text
Linux file handling is important in DevOps.
I am learning basic Linux commands.
Reading and writing files is useful for DevOps.
```

`cat` displays the complete contents of the file.

---

## 6. Read the First Two Lines Using `head`

### Command

```bash
head -n 2 notes.txt
```

### Output

```text
Linux file handling is important in DevOps.
I am learning basic Linux commands.
```

`head` displays the beginning of a file.

---

## 7. Read the Last Two Lines Using `tail`

### Command

```bash
tail -n 2 notes.txt
```

### Output

```text
I am learning basic Linux commands.
Reading and writing files is useful for DevOps.
```

`tail` displays the end of a file.

---

## Command Summary

| Command | Purpose                       |
| ------- | ----------------------------- |
| `touch` | Creates an empty file         |
| `echo`  | Prints text                   |
| `>`     | Writes/overwrites a file      |
| `>>`    | Appends text to a file        |
| `tee`   | Displays and writes text      |
| `cat`   | Reads the complete file       |
| `head`  | Reads the beginning of a file |
| `tail`  | Reads the end of a file       |

---

## DevOps Connection

File handling is a basic but important Linux skill.

In DevOps, we regularly work with:

* Application logs
* Configuration files
* Shell scripts
* Environment files
* System logs

For example, `cat`, `head`, and `tail` are commonly used to inspect files and logs while troubleshooting.

---

## Day 06 Takeaway

Today I practiced:

* Creating files with `touch`
* Writing files with `>`
* Appending data with `>>`
* Using `tee` to write and display data
* Reading complete files with `cat`
* Reading the beginning with `head`
* Reading the end with `tail`

**Day 06 completed ✅**

