## Windows Installation

Windows installation can be done using the python data science platform [Anaconda](https://www.anaconda.com/download/#macos). If not using Anaconda, Python3 can downloaded directly from the [Python3](https://www.python.org/downloads/) website.

#### Option one - Anaconda

1. Install [Anaconda](https://www.anaconda.com/download/#macos) with the latest version of Python.
2. Open the Anaconda app.
3. Launch [Spyder](https://www.spyder-ide.org/) in Anaconda.
4. Pull the FFC repository from Github using Git, or download the repository from the Github website.
5. Open Spyder, and open FFC as a project. 

```
git clone https://github.com/leogoesger/func-flow.git
cd func-flow
```

#### Option two - Install Python3

1. Install [Python3](https://www.python.org/downloads/), [Git](https://git-scm.com/download/win) and a [text editor](https://www.sublimetext.com/3) of your choice.
2. Add Python to your local [System Path](https://www.pythoncentral.io/add-python-to-path-python-is-not-recognized-as-an-internal-or-external-command/)

   * Locate `Python3` from your local computer. Python is usually located in the following folder:

     ```
     C:\python3
     ```

     or

     ```
     C:\Users\your-name\AppData\Local\Programs\Python\Python36-32
     ```

   * Follow the directions in this [link](https://www.pythoncentral.io/add-python-to-path-python-is-not-recognized-as-an-internal-or-external-command/) from step 2 to the end.

   * Enter the Command Prompt by typing `cmd` in the search bar, and type `python` in Command Prompt. You should see the following:

     ```
     Python 3.6.4 (v3.6.4:d48ecebad5, Dec 18 2017, 21:07:28)
     [GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
     Type "help", "copyright", "credits" or "license" for more information.
     >>>
     ```

   * Type `exit()` to exit the python shell.

3. Clone your project in [Command Prompt](http://www.informit.com/blogs/blog.aspx?uk=The-10-Most-Important-Linux-Commands) using Git:

   ```
   git clone https://github.com/leogoesger/func-flow.git
   cd func-flow
   ```

4. Create and activate a [virtual environment](https://docs.python.org/3/library/venv.html).

   ```
   python -m venv my-virtualenv
   my-virtualenv\Scripts\activate
   ```

5. Install dependencies using pip.

   ```
   pip install -r requirements.txt
   ```



