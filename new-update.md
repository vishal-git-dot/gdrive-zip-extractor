# 📦 ZIP File Extractor for Google Colab

A Google Colab-friendly Python script for extracting files from a ZIP archive directly to Google Drive.

The script provides detailed information about the ZIP archive, displays all files contained inside it, calculates file sizes, and allows the user to choose **what should be extracted**.

---

## ✨ Features

- 📦 Check whether the ZIP file exists
- 💾 Display the compressed ZIP file size
- 📋 List all files contained inside the ZIP
- 📊 Display the size of every file
- 📦 Calculate the total uncompressed size
- 🎯 Choose what to extract
- ⚡ Extract files in configurable chunks
- 📈 Show extraction progress using `tqdm`
- 🚀 Calculate extraction speed
- ⏱️ Display extraction time
- 📂 Extract directly to Google Drive
- 🎯 Select individual files by their displayed number

---

# 🆕 New Feature: Extraction Selection

The main update in this version is the ability to choose **which files should be extracted**.

After displaying the ZIP contents, the script now provides two options:

```text
1) 📦 Extract the entire ZIP file
2) 🎯 Extract selected files
```

## Option 1 — Extract the Entire ZIP

Selecting:

```text
1
```

will extract every file inside the ZIP archive.

Example:

```text
👉 Select extraction option (1 or 2): 1
```

The script automatically selects all files and extracts them.

You do not need to manually enter any file numbers.

---

## Option 2 — Extract Selected Files

Selecting:

```text
2
```

allows you to choose specific files.

The script displays every file with a number:

```text
[  1] file_a.txt
[  2] folder/file_b.jpg
[  3] video.mp4
[  4] dataset.csv
```

You can then enter the numbers of the files you want.

For example:

```text
🎯 Enter file numbers to extract (example: 1,3,7): 1,3
```

Only files **1 and 3** will be extracted.

You can select multiple files by separating their numbers with commas:

```text
1,3,7
```

---

# 🛠️ Requirements

This notebook is designed for:

- Google Colab
- Google Drive
- Python 3

The script uses the following Python modules:

```python
import zipfile
import os
import time
from tqdm.auto import tqdm
```

`zipfile`, `os`, and `time` are part of Python's standard library.

`tqdm` is commonly available in Google Colab.

If `tqdm` is not available, install it with:

```python
!pip install tqdm
```

---

# 📂 Google Drive Setup

Before running the extraction code, make sure your Google Drive is mounted in Colab.

Use:

```python
from google.colab import drive

drive.mount('/content/drive')
```

After mounting Google Drive, your Drive is generally available at:

```text
/content/drive/
```

For example, a ZIP file stored in:

```text
My Drive/my_archive.zip
```

would normally have a path similar to:

```text
/content/drive/MyDrive/my_archive.zip
```

---

# ⚙️ Configuration

The most important configuration variables are:

```python
ZIP_FILE = "/content/drive/.zip"

OUTPUT_DIR = "/content/drive/"

CHUNK_SIZE = 16 * 1024 * 1024
```

## ZIP_FILE

Set this to the **full path of your ZIP file**.

Example:

```python
ZIP_FILE = "/content/drive/MyDrive/my_archive.zip"
```

Do not leave it as:

```python
ZIP_FILE = "/content/drive/.zip"
```

unless that is actually the location and filename of your ZIP archive.

---

## OUTPUT_DIR

This determines where the extracted files will be stored.

Example:

```python
OUTPUT_DIR = "/content/drive/"
```

You can also use a dedicated folder:

```python
OUTPUT_DIR = "/content/drive/MyDrive/extracted_files/"
```

---

## CHUNK_SIZE

The extraction process reads the ZIP file in chunks.

Current value:

```python
CHUNK_SIZE = 16 * 1024 * 1024
```

This equals:

```text
16 MB
```

Using chunks prevents the script from trying to load an entire large file into memory at once.

---

# 🔄 How the Script Works

The extraction process follows these steps:

```text
1. Check ZIP file
       ↓
2. Get ZIP size
       ↓
3. Open ZIP
       ↓
4. List files inside ZIP
       ↓
5. Display individual file sizes
       ↓
6. Calculate total uncompressed size
       ↓
7. Ask user what to extract
       ↓
   ┌───────────────────────────┐
   │ 1. Entire ZIP             │
   │ 2. Selected files         │
   └───────────────────────────┘
       ↓
8. Confirm selected files
       ↓
9. Extract selected files
       ↓
10. Show progress
       ↓
11. Calculate extraction speed
       ↓
12. Display final summary
```

---

# 📋 ZIP Contents Display

Before extraction, the script lists all non-directory files inside the ZIP.

Example:

```text
================================================================================
📦 FILES INSIDE ZIP
================================================================================
[  1]  dataset/file1.csv
       📄 Size  : 500.00 MB    ➜    📦 Total so far : 500.00 MB

[  2]  dataset/file2.csv
       📄 Size  : 750.00 MB    ➜    📦 Total so far : 1.22 GB

[  3]  images/image.zip
       📄 Size  : 2.50 GB    ➜    📦 Total so far : 3.72 GB
================================================================================
```

The number shown beside each file is important when using:

```text
2) 🎯 Extract selected files
```

For example:

```text
[  1] file1.csv
[  2] file2.csv
[  3] file3.csv
```

Entering:

```text
1,3
```

will extract:

```text
file1.csv
file3.csv
```

---

# 🎯 File Selection

When the user selects option `2`, the script asks:

```text
🎯 Enter file numbers to extract (example: 1,3,7):
```

### Example

Suppose the ZIP contains:

```text
[  1] document.pdf
[  2] image.jpg
[  3] video.mp4
[  4] dataset.csv
[  5] backup.tar
```

If the user enters:

```text
1,4
```

the script extracts:

```text
document.pdf
dataset.csv
```

Only those files are extracted.

---

# ⚠️ Invalid File Numbers

If the user enters a number that does not exist, the script ignores it.

For example:

```text
🎯 Enter file numbers to extract (example: 1,3,7): 1,3,99
```

If there are only 5 files, the script will display:

```text
⚠️ Ignoring invalid number: 99
```

The valid files will still be selected.

---

# ⚠️ Invalid Input

If the user enters something that is not a number:

```text
1,hello,3
```

the script ignores the invalid value:

```text
⚠️ Ignoring invalid input: hello
```

The valid selections are still processed.

---

# 📊 Selected File Confirmation

Before extraction starts, the script displays a summary of the selected files.

Example:

```text
================================================================================
🎯 SELECTED FILES
================================================================================
[  1] 📄 dataset/file1.csv
       💾 Size : 500.00 MB
       📦 Selected total so far : 500.00 MB

[  2] 📄 dataset/file3.csv
       💾 Size : 2.00 GB
       📦 Selected total so far : 2.49 GB

================================================================================
📦 Files selected : 2
💾 Total selected : 2.49 GB
================================================================================
```

This makes it easy to verify what will be extracted before the extraction actually begins.

---

# ⚡ Extraction Progress

Each selected file is extracted using chunks.

The progress bar shows the extraction status:

```text
⚡ Progress: 100%|██████████| 2.00G/2.00G [01:32<00:00, 22.5MB/s]
```

After the file finishes, the script displays the extraction speed and elapsed time.

Example:

```text
✅ Complete | ⚡ 22.5 MB/s | ⏱️ 1.5 min
```

---

# 📈 Extraction Speed

The script calculates the extraction speed using:

```python
speed = (
    info.file_size
    / elapsed
    / (1024 ** 2)
)
```

The resulting speed is displayed in MB/s.

For example:

```text
⚡ 35.4 MB/s
```

The actual speed depends on factors such as:

- ZIP compression
- ZIP file size
- Google Drive performance
- Colab runtime performance
- CPU performance
- Storage performance
- Compression format
- File type

---

# ⏱️ Total Extraction Time

After all selected files are extracted, the script displays the total extraction time.

Example:

```text
⏱️ Total time      : 12.35 minutes
```

---

# 📂 Output Location

Extracted files are written to:

```python
OUTPUT_DIR
```

For example:

```python
OUTPUT_DIR = "/content/drive/"
```

If the ZIP contains:

```text
folder/file.txt
```

the script creates the corresponding directory structure:

```text
/content/drive/folder/file.txt
```

The original folder structure stored inside the ZIP is preserved.

---

# 🔐 Important Security Note

The script currently constructs the output path using:

```python
output_path = os.path.join(
    OUTPUT_DIR,
    info.filename
)
```

For ZIP files from untrusted sources, ZIP entries should ideally be validated to prevent unsafe paths such as:

```text
../../some_file
```

Only use ZIP archives you trust, or add ZIP path-validation before extracting untrusted archives.

---

# 🚀 Complete Notebook Code

The following is the complete updated code.

Copy the entire code into a Google Colab cell.

```python
import zipfile
import os
import time
from tqdm.auto import tqdm

# ============================================================
# CONFIGURATION
# ============================================================

# IMPORTANT:
# Put the FULL PATH to your ZIP file here.
#
# Example:
# ZIP_FILE = "/content/drive/MyDrive/my_archive.zip"

ZIP_FILE = "/content/drive/.zip"

# Extract to Google Drive
OUTPUT_DIR = "/content/drive/"

CHUNK_SIZE = 16 * 1024 * 1024  # 16 MB


# ============================================================
# HELPER FUNCTIONS
# ============================================================

def format_size(size_bytes):
    """
    Convert bytes into a readable GB / MB / KB format.
    """

    if size_bytes >= 1024 ** 3:
        return f"{size_bytes / (1024 ** 3):.2f} GB"

    elif size_bytes >= 1024 ** 2:
        return f"{size_bytes / (1024 ** 2):.2f} MB"

    elif size_bytes >= 1024:
        return f"{size_bytes / 1024:.2f} KB"

    else:
        return f"{size_bytes} B"


# ============================================================
# CHECK ZIP FILE
# ============================================================

if not os.path.isfile(ZIP_FILE):
    raise FileNotFoundError(
        f"\n❌ ZIP file not found:\n{ZIP_FILE}\n\n"
        "Please set ZIP_FILE to the full path of your .zip file."
    )


# ============================================================
# GET ZIP FILE SIZE
# ============================================================

zip_file_size = os.path.getsize(ZIP_FILE)


print("\n" + "=" * 80)
print("📦 ZIP FILE INFORMATION")
print("=" * 80)

print(f"📄 ZIP file : {ZIP_FILE}")
print(f"💾 ZIP size : {format_size(zip_file_size)}")

print("=" * 80)


# ============================================================
# SHOW ZIP CONTENTS
# ============================================================

with zipfile.ZipFile(ZIP_FILE, "r") as zip_ref:

    all_files = [
        f for f in zip_ref.infolist()
        if not f.is_dir()
    ]

    print("\n" + "=" * 80)
    print("📦 FILES INSIDE ZIP")
    print("=" * 80)

    # Running total of ALL files inside ZIP
    running_total = 0

    for i, file in enumerate(all_files, 1):

        # Add current file size
        running_total += file.file_size

        individual_size = format_size(file.file_size)
        cumulative_size = format_size(running_total)

        print(
            f"[{i:3}]  "
            f"{file.filename}"
        )

        print(
            f"       📄 Size  : {individual_size}"
            f"    ➜    📦 Total so far : {cumulative_size}"
        )

    print("=" * 80)

    # --------------------------------------------------------
    # TOTAL UNCOMPRESSED SIZE
    # --------------------------------------------------------

    total_uncompressed_size = sum(
        file.file_size for file in all_files
    )

    print(
        f"📦 Total files inside ZIP : {len(all_files)}"
    )

    print(
        f"💾 Total uncompressed data: "
        f"{format_size(total_uncompressed_size)}"
    )

    print(
        f"🗜️ Actual ZIP file size   : "
        f"{format_size(zip_file_size)}"
    )

    print("=" * 80)


    # ========================================================
    # SELECT EXTRACTION MODE
    # ========================================================

    print("\n" + "=" * 80)
    print("🎯 EXTRACTION OPTIONS")
    print("=" * 80)

    print("1) 📦 Extract the entire ZIP file")
    print("2) 🎯 Extract selected files")

    print("=" * 80)


    # --------------------------------------------------------
    # ASK USER FOR EXTRACTION MODE
    # --------------------------------------------------------

    while True:

        extraction_mode = input(
            "\n👉 Select extraction option (1 or 2): "
        ).strip()

        if extraction_mode in ["1", "2"]:
            break

        print(
            "⚠️ Invalid option. Please enter 1 or 2."
        )


    # ========================================================
    # OPTION 1: EXTRACT ENTIRE ZIP
    # ========================================================

    if extraction_mode == "1":

        selected_files = all_files.copy()

        print("\n" + "=" * 80)
        print("📦 ENTIRE ZIP SELECTED")
        print("=" * 80)

        print(
            f"📦 Files to extract : "
            f"{len(selected_files)}"
        )

        print(
            f"💾 Total data       : "
            f"{format_size(total_uncompressed_size)}"
        )

        print("=" * 80)


    # ========================================================
    # OPTION 2: SELECT FILES
    # ========================================================

    else:

        selection = input(
            "\n🎯 Enter file numbers to extract "
            "(example: 1,3,7): "
        )

        selected_numbers = []

        for x in selection.split(","):

            x = x.strip()

            if not x:
                continue

            try:
                selected_numbers.append(int(x))

            except ValueError:
                print(
                    f"⚠️ Ignoring invalid input: {x}"
                )


        # ----------------------------------------------------
        # BUILD SELECTED FILE LIST
        # ----------------------------------------------------

        selected_files = []

        for number in selected_numbers:

            if 1 <= number <= len(all_files):

                selected_files.append(
                    all_files[number - 1]
                )

            else:

                print(
                    f"⚠️ Ignoring invalid number: {number}"
                )


# ============================================================
# CONFIRM SELECTION
# ============================================================

print("\n" + "=" * 80)

if extraction_mode == "1":
    print("🎯 FILES TO BE EXTRACTED")
else:
    print("🎯 SELECTED FILES")

print("=" * 80)


total_selected_size = 0


for i, file in enumerate(selected_files, 1):

    # Add selected file size
    total_selected_size += file.file_size

    print(
        f"[{i:3}] 📄 {file.filename}"
    )

    print(
        f"       💾 Size : {format_size(file.file_size)}"
    )

    print(
        f"       📦 Selected total so far : "
        f"{format_size(total_selected_size)}"
    )

    print()


print("=" * 80)

print(
    f"📦 Files selected : "
    f"{len(selected_files)}"
)

print(
    f"💾 Total selected : "
    f"{format_size(total_selected_size)}"
)

print("=" * 80)


# ============================================================
# HANDLE EMPTY SELECTION
# ============================================================

if not selected_files:

    print(
        "\n❌ No files selected."
    )

    print(
        "⚠️ Extraction cancelled."
    )

    raise SystemExit


# ============================================================
# CREATE OUTPUT DIRECTORY
# ============================================================

os.makedirs(
    OUTPUT_DIR,
    exist_ok=True
)


# ============================================================
# EXTRACT SELECTED FILES
# ============================================================

with zipfile.ZipFile(ZIP_FILE, "r") as zip_ref:

    overall_start = time.time()

    for index, info in enumerate(selected_files, 1):

        output_path = os.path.join(
            OUTPUT_DIR,
            info.filename
        )

        # ----------------------------------------------------
        # CREATE PARENT DIRECTORIES
        # ----------------------------------------------------

        parent_directory = os.path.dirname(
            output_path
        )

        if parent_directory:

            os.makedirs(
                parent_directory,
                exist_ok=True
            )


        # ----------------------------------------------------
        # EXTRACTION INFORMATION
        # ----------------------------------------------------

        print("\n" + "─" * 80)

        print(
            f"🚀 EXTRACTING {index}/{len(selected_files)}"
        )

        print(
            f"📄 {info.filename}"
        )

        print(
            f"💾 Size : "
            f"{format_size(info.file_size)}"
        )

        print("─" * 80)


        start = time.time()


        # ----------------------------------------------------
        # EXTRACT FILE IN CHUNKS
        # ----------------------------------------------------

        with zip_ref.open(info, "r") as source:

            with open(
                output_path,
                "wb"
            ) as target:

                with tqdm(
                    total=info.file_size,
                    unit="B",
                    unit_scale=True,
                    unit_divisor=1024,
                    desc="⚡ Progress",
                    colour="green",
                    dynamic_ncols=True
                ) as progress:

                    while True:

                        chunk = source.read(
                            CHUNK_SIZE
                        )

                        if not chunk:
                            break

                        target.write(
                            chunk
                        )

                        progress.update(
                            len(chunk)
                        )


        # ----------------------------------------------------
        # CALCULATE SPEED
        # ----------------------------------------------------

        elapsed = time.time() - start


        if elapsed > 0:

            speed = (
                info.file_size
                / elapsed
                / (1024 ** 2)
            )

        else:

            speed = 0


        print(
            f"✅ Complete | "
            f"⚡ {speed:.1f} MB/s | "
            f"⏱️ {elapsed / 60:.1f} min"
        )


# ============================================================
# DONE
# ============================================================

total_time = time.time() - overall_start


print("\n")

print("=" * 80)
print("🎉 SELECTED FILES EXTRACTED SUCCESSFULLY!")
print("=" * 80)

print(
    f"📦 Files extracted : "
    f"{len(selected_files)}"
)

print(
    f"💾 Total data      : "
    f"{format_size(total_selected_size)}"
)

print(
    f"⏱️ Total time      : "
    f"{total_time / 60:.2f} minutes"
)

print(
    f"📂 Location        : "
    f"{OUTPUT_DIR}"
)

print("=" * 80)
```

---

# 📝 Example Usage

After mounting Google Drive and setting the ZIP path:

```python
ZIP_FILE = "/content/drive/MyDrive/my_archive.zip"
```

run the notebook.

The script might show:

```text
================================================================================
🎯 EXTRACTION OPTIONS
================================================================================
1) 📦 Extract the entire ZIP file
2) 🎯 Extract selected files
================================================================================

👉 Select extraction option (1 or 2):
```

## Extract Everything

Enter:

```text
1
```

The script extracts every file.

---

## Extract Specific Files

Enter:

```text
2
```

Then:

```text
🎯 Enter file numbers to extract (example: 1,3,7): 2,5,8
```

The script extracts only files:

```text
2
5
8
```

---

# 💡 Why This Feature Is Useful

The new selection functionality is especially useful when working with **large ZIP archives**.

For example, imagine a ZIP archive contains:

```text
video_01.mp4      5 GB
video_02.mp4      6 GB
video_03.mp4      7 GB
video_04.mp4      8 GB
dataset.csv       2 GB
```

The complete archive may contain:

```text
28 GB
```

If you only need:

```text
dataset.csv
video_01.mp4
```

you no longer need to extract all 28 GB.

You can select the corresponding file numbers and extract only the required files.

This can save:

- 💾 Storage space
- ⏱️ Extraction time
- 📂 Unnecessary files
- 🔄 Repeated extraction work

---

# 📌 Summary

This updated version supports two extraction modes:

| Option | Function |
|---|---|
| `1` | Extract every file in the ZIP |
| `2` | Extract only selected files |

The new feature makes the ZIP extractor more flexible, particularly when working with large archives where extracting everything is unnecessary.

---

## 👨‍💻 Author

ZIP extraction utility designed for use with **Google Colab + Google Drive**.
