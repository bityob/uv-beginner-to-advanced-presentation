## uv: Speed Meets Simplicity in Python Package Management

**RND Light-Talk**, December 2024

[bit.ly/uv-intro](https://bit.ly/uv-intro)

<grid drag="40 30" drop="bottomleft">
Yonatan Bitton [@bityob](https://linktr.ee/bityob)
</grid>

<grid drag="40 30" drop="bottomright">
![[Pasted image 20241224201606.png|150]]
</grid>




---

### The Painful Past of Python Packaging

+ Python packaging can be hard for beginners:
    - Bootstrapping, i.e., how to even get started!
    - Activation, i.e., how do virtual environments (venvs) in Python work?
+ Managing Python versions
+ Handling "dependency hell"
+ No unified tool for all workflows
+ pip is very, very slow
	

---

### Meet **uv**

An **extremely fast** Python package and project manager, built with the power of **Rust**.

---

### A fast, all-in-one Python package manager

+ You can use uv to: install Python, create virtual environments, resolve dependencies, install packages, build packages and more
+ A drop-in alternative to `pip`, `pipx`, `pyenv`, `virtualenv`, `poetry` and more
+ A single static binary that gives you everything you need to be productive with Python
+ 10-100x faster than alternatives. Written in Rust

---

### New Tool, Big Impact

+ 21+ million downloads last month
+ 33.3k stars in GitHub
+ 6.8+ billion packages were downloaded using UV last month, which is 13% of total downloads

---
![[uv-beginner-to-advanced-presentation-images/Pasted image 20241223171715-Photoroom.png]]

---

### Agenda

+ Overview
	- Installation
	- pip interface
	- uv commands
+ The uv ecosystem
	- Projects
	- Tools
	- Scripts
+ Why UV is so fast?

---

### Installation 

- No need to have Python/Rust installed to install UV

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

- Alternatively, can be installed using `pip` or `pipx`

```bash
pip install uv
pipx install uv
```

---

### pip interface

+ Drop-in replacement for common `pip` and `virtualenv` commands
+ Creating a virtual environment: `uv venv`
+ Install a package: `uv pip install flask`
+ List packages: `uv pip freeze`

---


```python [1-4|5-14|15-22]
$ uv venv
Using CPython 3.10.14
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ uv pip install flask
Resolved 7 packages in 491ms
Installed 7 packages in 13ms
 + blinker==1.9.0
 + click==8.1.8
 + flask==3.1.0
 + itsdangerous==2.2.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + werkzeug==3.1.3
$ uv pip freeze
blinker==1.9.0
click==8.1.8
flask==3.1.0
itsdangerous==2.2.0
jinja2==3.1.5
markupsafe==3.0.2
werkzeug==3.1.3
```


---

### uv commands

<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://florianbrand.de/posts/uv-intro#usage">Source: https://florianbrand.de/posts/uv-intro#usage</a><!-- element class="fragment" -->

---

### uv commands


|uv command|pip command(s)|Notes|
|---|---|---|
|`uv init <name>`|N/A|creates a new empty project according to the `pyproject.toml` specification|


---

### uv commands

|uv command|pip command(s)|Notes|
|---|---|---|
|`uv add <package>`|`pip install <package>`|uv also adds it to the `pyproject.toml` and update the `uv.lock` file|

---

Since we are using docker to run our applications, you can't run `uv add <package>` on local, since it will try to **install the packages too.**

For such cases, just run with `--no-sync` flag

```
uv add <package> --no-sync
```

---

### uv commands

|uv command|pip command(s)|Notes|
|---|---|---|
|`uv lock`|`N/A`|creates the `uv.lock` file with all pinned dependencies|

---

### uv commands

|uv command|pip command(s)|Notes|
|---|---|---|
|`uv sync`|`pip freeze \| xargs pip uninstall -y && pip install -r requirements.txt`|uv also creates a venv if it doesn't exist|

---

### uv commands

|uv command|pip command(s)|Notes|
|---|---|---|
|`uv run <python_file>`|`source .venv/activate && python <python_file>`|uv downloads python if not exists and sync the venv before run |

---


### uv commands

|uv command|pip command(s)|Notes|
|---|---|---|
|`uv remove <package>`|`pip uninstall <package> && pip freeze > requirements.txt`||

---

<!--<div style="width: 150%; font-size: 150%"> -->

```python [1-2|3-10|11-14|15-24]
$ uv init
Initialized project `playground`
$ cat pyproject.toml
[project]
name = "playground"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []
$ uv add pycowsay
Resolved 2 packages in 346ms
Installed 1 package in 7ms
 ++ pycowsay==0.0.0.2
$ uv run pycowsay "Hello Python World!"

  -------------------
< Hello Python World! >
  -------------------
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||
```

<!--</div>-->

---

### uv commands


|uv command|pip command(s)|Notes|
|---|---|---|
|`uv python list`|N/A|list all python versions installed on your **computer**|
|`uv python install <version>`|N/A|using precompiled binaries|

---

<!--<div style="width: 150%; font-size: 150%"> -->

```python [1-11|12-14]
$ uv python list
cpython-3.13.0a0-linux-x86_64-gnu               /usr/local/bin/python3.13
cpython-3.12.7-linux-x86_64-gnu                 <download available>
cpython-3.12.0-linux-x86_64-gnu                 /home/linuxbrew/.linuxbrew/opt/python@3.12/bin/python3.12
cpython-3.11.10-linux-x86_64-gnu                <download available>
cpython-3.11.6-linux-x86_64-gnu                 /home/linuxbrew/.linuxbrew/opt/python@3.11/bin/python3.11
cpython-3.11.4-linux-x86_64-gnu                 /home/bit/.asdf/installs/python/3.11.4/bin/python3.11
cpython-3.11.4-linux-x86_64-gnu                 /home/bit/.asdf/installs/python/3.11.4/bin/python3 -> python3.11
cpython-3.11.4-linux-x86_64-gnu                 /home/bit/.asdf/installs/python/3.11.4/bin/python -> python3.11
cpython-3.10.15-linux-x86_64-gnu                <download available>
...
$ uv python install cpython-3.10.15-linux-x86_64-gnu
Installed Python 3.10.15 in 4.74s
 + cpython-3.10.15-linux-x86_64-gnu
```

<!--</div>-->

---

### uv ecosystem

---

### Projects 

- Python projects/repositories
- Using with `uv` commands like previous slides
- Configuration file is inside `pyproject.toml`

---

### Tools

- CLI tools, e.g `ruff`, `llm`, `pycowsay`
- No file needed
- Virtual environment included
- Same like `pipx` tool

```
uvx <tool>
uv tool run <tool>
```

```
$ uvx art text "Hello World!"
 _   _        _  _         __        __              _      _  _
| | | |  ___ | || |  ___   \ \      / /  ___   _ __ | |  __| || |
| |_| | / _ \| || | / _ \   \ \ /\ / /  / _ \ | '__|| | / _` || |
|  _  ||  __/| || || (_) |   \ V  V /  | (_) || |   | || (_| ||_|
|_| |_| \___||_||_| \___/     \_/\_/    \___/ |_|   |_| \__,_|(_)
```

---

### Scripts

- Scripts are single files without project files
- A script without dependencies is easy, just `uv run script` 
- But how can we run a script with dependencies??
+ uv supports specifying dependencies **on invocation**

---

<!--<div style="width: 150%; font-size: 150%"> -->

```python [1-12|14-19]
$ cat http_requests_uuid.py
import concurrent.futures

urls = ["https://httpbin.org/uuid" for _ in range(5)]

def get_url(url):
    import requests
    return requests.get(url).json()

with concurrent.futures.ThreadPoolExecutor(3) as executor:
    for x in executor.map(get_url, urls):
        print(x)

$ uv run --with requests http_requests_uuid.py
{'uuid': '2f9c8816-960f-40e8-a26e-a0ada4b4c5cc'}
{'uuid': '22d43f6c-7f81-46d1-867c-43260b0155bb'}
{'uuid': '3303265d-1d9c-41ac-a43e-5a9fed0aa02c'}
{'uuid': 'bed4fdd9-0e7b-4641-b9ed-19889e435c42'}
{'uuid': '6944a068-67d9-4778-bb40-2b77c9115e5e'}
```

<!--</div>-->

---

But this can be done even better

---

![[uv-beginner-to-advanced-presentation-images/Pasted image 20241223185623.png]]

https://peps.python.org/pep-0723/


---

Now we can use something like this 

<!--<div style="width: 150%; font-size: 150%"> -->

```python [1-2|3-9|3-20|21-27]
$ uv add --script http_requests_uuid.py requests
Updated `http_requests_uuid.py`
$ cat http_requests_uuid.py
# /// script
# requires-python = ">=3.10"
# dependencies = [
#     "requests",
# ]
# ///
import concurrent.futures

urls = ["https://httpbin.org/uuid" for _ in range(5)]

def get_url(url):
    import requests
    return requests.get(url).json()

with concurrent.futures.ThreadPoolExecutor(3) as executor:
    for x in executor.map(get_url, urls):
        print(x)
$ uv run http_requests_uuid.py
Reading inline script metadata from `http_requests_uuid.py`
{'uuid': '6d852239-642d-46e1-9718-0f0862808b91'}
{'uuid': 'b70dfc29-5cbc-4f36-9476-fd1c79ec32c0'}
{'uuid': 'c790752b-681a-45d2-af17-38fb44917612'}
{'uuid': 'd22336bb-cd53-40d9-b1cb-c2348cdcdac6'}
{'uuid': '81e9cda6-6072-4fd8-80b0-08cc55553fd5'}
```

<!--</div>-->

---

You can limit uv to only considering distributions released before a specific date

<!--<div style="width: 150%; font-size: 150%"> -->

```python [7|13-15]
$ cat example_requests.py
# /// script
# dependencies = [
#   "requests",
# ]
# [tool.uv]
# exclude-newer = "2020-10-16T00:00:00Z"
# ///

import requests

print(requests.__version__)
$ uv run example_requests.py
Reading inline script metadata from `example_requests.py`
2.24.0
```

<!--</div>-->


<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://docs.astral.sh/uv/guides/scripts/">Source: https://docs.astral.sh/uv/guides/scripts/</a>
</div><!-- element class="fragment" -->


---

Now, the interesting part...

### Why UV is so fast?<!-- element class="fragment" -->

---

### (1)

Written in pure **Rust**.

100k~ lines of code.

But... uv has gotten significantly faster over time <!-- element class="fragment" -->
 
---

### (2)

Fast python install using precompiled binaries.

![[uv-beginner-to-advanced-presentation-images/Pasted image 20241223235115.png]] <!-- element class="fragment" -->

Really, there are around 400 builds (including checksum files), but still! <!-- element class="fragment" -->

---

### (3)

Version parsing.

+ `1.0.0`
+ `2.3.1-beta.1`
+ `1.0.post345`
+ `1.2.3.4.5-a8.post9`

Representing these is pretty hard. <!-- element class="fragment" -->

<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://youtu.be/gSKTfG1GXYQ?t=1544">Source: https://youtu.be/gSKTfG1GXYQ?t=1544</a>
</div><!-- element class="fragment" -->

---

The full representation of this is

```rust
struct VersionFull {
    epoch: u64,
    release: Vec<u64>,
    pre: Option<Prerelease>,
    post: Option<u64>,
    dev: Option<u64>,
    local: LocalVersion,
    min: Option<u64>,
    max: Option<u64>,
}
```

Notes:
- Vectors, memory allocation, release segments, 

---

Apparently, we can represent over 90% of versions with a single u64.

```rust
struct VersionSmall(u64);
```

![[uv-beginner-to-advanced-presentation-images/Pasted image 20241224010957.png]] <!-- element class="fragment" -->

<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
Credit to Andrew Gallant (@BurntSushi)
</div><!-- element class="fragment" -->

---
### And the Best Part:

+ Greater versions map to larger integers 
	→ Simplifies comparison
+  Version comparison becomes extremely fast

---

![[uv-beginner-to-advanced-presentation-images/Pasted image 20241224021528.png]]

---

### (4) 

### Reading package `METADATA`

+ Built distributions (wheels) are packaged as ZIP archives
+ Somewhere within the ZIP file, there's a `METADATA` file
+ Some registries expose the `METADATA` file directly, while others do not
+ In such cases, we may need to run Python code to determine the package dependencies 🙁

<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://youtu.be/gSKTfG1GXYQ?t=1884">Source: https://youtu.be/gSKTfG1GXYQ?t=1884</a>
</div><!-- element class="fragment" -->

---

**Goal**: Avoid downloading the entire ZIP archive or running Python code just to read the `METADATA` file and determine package dependencies.

---

![[uv-beginner-to-advanced-presentation-images/Pasted image 20241224013805.png]]


<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://en.wikipedia.org/wiki/ZIP_(file_format)">Source: Wikipedia</a>
</div><!-- element class="fragment" -->

---

- HTTP Range request the Central Directory
+ Locate the `METADATA` file by reading the Central Directory
+ HTTP Range request the `METADATA` file

```
GET /example.whl HTTP/1.1
Host: example.com
Range: bytes=-100
```
<!-- element class="fragment" -->

---

### (5)

### Cache design

+ **Global cache of unpacked archives:** uv uses aggressive caching to avoid re-downloading (and re-building) dependencies that have already been accessed in prior runs
+ **Most installs are just:** hardlink a bunch of files from one place to another
+ Really fast and very space efficient
+ This only applies for files, not for metadata

---

For metadata, there is another **last** hack called,

**Zero-copy deserialization.**

---

### (6)

### Zero-copy deserialization

A technique that reduces the time and memory required to access and use data by **directly referencing bytes in the serialized form**


<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://rkyv.org/zero-copy-deserialization.html">Source: https://rkyv.org/zero-copy-deserialization.html</a>
</div><!-- element class="fragment" -->

---

Simple zero-copy

```rust
{ "quote": "I don't know, I didn't listen." }
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
<!-- element class="fragment" -->

```rust
struct NonZeroCopy {
    quote: String,
}

struct ZeroCopy<'a> {
    quote: &'a str,
}
```
<!-- element class="fragment" -->

---

Total zero-copy


![[uv-beginner-to-advanced-presentation-images/Pasted image 20241224024718.png]]

```rust
struct Example {
  quote: String,
  a: [u8; 12],
  b: u64,
  c: char,
}
```
<!-- element class="fragment" -->


---

- On JSON, first you read the file from disc, then run a parser, and then you grab the contents and put them in the struct. All these operations would get **slower and slower as the data got bigger**
- Instead, the **zero-copy** the operation, doesn't **scale** with our data. No matter how much or how little data we have

---

On UV, metadata is stored on disk in the same format as its in-memory JSON representation. This allows you to simply read it from the disk without needing to reparse it or allocate additional memory.

<div style="position: fixed; bottom: 0; width: 100%; text-align: center; font-size: 70%; color: gray;">
<a href="https://youtu.be/gSKTfG1GXYQ?t=2246">Source: https://youtu.be/gSKTfG1GXYQ?t=2246</a>
</div>

---

### Takeaways

+ **Blazing Speed**: uv delivers swift package management and environment setup.
+ **Innovative Approach**: Focused on speed and efficiency, uv redefines Python tooling.
+ **Try It Now**: Explore uv’s potential to elevate your Python development.

---


### Questions?


---

### Sources 

- Sane Python dependency management with uv ([florianbrand.de](https://florianbrand.de/posts/uv-intro))
- Charlie Marsh (founder of Astral) Presentation on Jane Street ([youtube](https://www.youtube.com/watch?v=gSKTfG1GXYQ&t=3s&ab_channel=JaneStreet))
- uv: Unified Python packaging ([astral.sh](https://astral.sh/blog/uv-unified-python-packaging))
- uv: Python packaging in Rust ([astral.sh](https://astral.sh/blog/uv))

---

### Learn More (1)

- pip vs. uv: How Streamlit Cloud sped up app load times by 55% ([blog.streamlit.io](https://blog.streamlit.io/content/images/size/w1200/2024/07/_Users_lacosta_Downloads_Announcement-20-3-.svg.png))
- GitHub - `astral-sh/uv`: An extremely fast Python package and project manager, written in Rust ([github](https://github.com/astral-sh/uv))
- Python Packaging is Great Now: `uv` is all you need ([dev.to](https://dev.to/astrojuanlu/python-packaging-is-great-now-uv-is-all-you-need-4i2d))
- Python Packaging Is Good Now (2016) ([blog.glyph.im](https://blog.glyph.im/2016/08/python-packaging.html))

---

### Learn More (2)

- Poetry versus uv ([loopwerk.io](https://www.loopwerk.io/articles/2024/python-uv-revisited/))
- Trying out PDM (and comparing it with Poetry and uv) ([loopwerk.io](https://www.loopwerk.io/articles/2024/trying-pdm/))
- Revisiting uv ([loopwerk.io](https://www.loopwerk.io/articles/2024/python-uv-revisited/))
- How to migrate your Poetry project to uv ([loopwerk.io](https://www.loopwerk.io/articles/2024/migrate-poetry-to-uv/))
- Zero-copy deserialization ([rkyv.org](https://rkyv.org/zero-copy-deserialization.html))
- uv docs ([astral.sh](https://docs.astral.sh/uv/))
- Nice uv cheatsheet ([dev.to](https://dev.to/codemaker2015/introducing-uv-next-gen-python-package-manager-4jbc))

