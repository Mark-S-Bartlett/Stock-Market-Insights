Jupyter Lab and Notebooks
=========

Computational notebooks combine code, results, text and images in a single document, yielding what Stephen Wolfram, creator of the Mathematica software package, has called a “computational essay”. And whether written using Jupyter, Mathematica, RStudio or any other platform, researchers can use them for iterative data exploration, communication, teaching and more.
- https://www.nature.com/articles/d41586-021-01174-w
- https://www.nature.com/articles/d41586-018-07196-1
- https://www.theatlantic.com/science/archive/2018/04/the-scientific-paper-is-obsolete/556676/
- https://www.nature.com/articles/515151a
- https://florianwilhelm.info/2018/11/working_efficiently_with_jupyter_lab/
- https://towardsdatascience.com/jupyter-notebook-or-lab-or-vs-code-b772f8388911

Command	Shortcut
Enter Command Mode	Esc
Run Cell	Ctrl Enter
Run Cell & Select Next	Shift Enter
Add Cell Above/Below	A / B
Copy/Cut/Paste Cell	C / X / V
Look Around Up/Down	Alt ⇧ / ⇩
Markdown Cell	M
Code Cell	Y
Delete Cell Output	M, Y (workaround)
Delete Cell	D D
Toggle Line Numbers	Shift L
Comment Line	Ctrl /
Command Palette	Accel Shift C
File Explorer	Accel Shift F
Toggle Bar	Accel B
Fullscreen Mode	Accel Shift D
Close Tab	Ctrl Q
Launcher	Accel Shift L

# Shortcuts

Shift + Enter to run a cell (code or markdown):
- A to insert a new cell above the current cell.
- B to insert a new cell below the current cell.
- M to change the current cell to Markdown
- Y to change the current cell to code.
- D + D (twice) to delete the selected cells.

# Magic commands

- Time the execution of a single line of code or the whole cell

```
# Run the code multiple times and find mean runtime 
%timeit CODE_LINE 
%%timeit CODE_CELL 
# Run once and report 
%time CODE_LINE 
%%time CODE_CELL
```

- Use `!` prefix to run a single bash command line

- `%%bash` to change the current code cell to run to bash mode, basically writing bash commands in there

```
%%bash
echo "hello from $BASH"
```

- `%%js`, `%% html`, `%%latex`, `%%python2`, `%%python3`, ... run and render code cells in that specified language or format.
- `autoreload` IPython extension is exceptionally helpful when you do not want to worry about reloading modules before executing new code. In other words when you change something in a certain module the current notebook uses, changes will take place when you run new code cells without having to worry about anything.

```
%load_ext autoreload
%autoreload 2
```

- Embedd tensorboard in jupyter notebook

```
%load_ext tensorboard
%tensorboard --logdir logs/model_training_logs
```

- Finally you can list all available magics by running %lsmagic, this will show both line and cell magics currently defined.

## Other
- Sometimes you will have this memory hungry variable, you can reclaim memory by setting it to `NONE` and then forcing gc to run

```
some_var = None
gc.collect()
```
- Use `sudo service jupyter restart` to restart jupyter because every once in a while jupyter would throw a fit and restarting kernels will not be enough to get it back to a responsive state.
- Add `?` before almost any function, variable, ... and run the code cell to access its documentation.
- `tqdm` which means "progress" in Arabic (taqadum, تقدّم) is not really related to jupyter notebooks but it can be used to show a smart progress meter. Just by wrapping any `iterabl`e with `tqdm(iterable)`

```
from tqdm import tqdm
for i in tqdm(range(10000)):
    pass
```

- When you want to count the number of files in a directory you can run the following command
```
!ls DIR_NAME | wc -l
```

## Classic notebooks extensions
For the classic notebook interface (which doesn't work with jupyter lab) various extension are found in jupyter_contrib_nbextensions. Howeve, one should be using Jupyter lab. Nevertheless, for the traditional jupyter notebook install jupyter_contrib_nbextensions and then install various useful extensions.

```
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
```
Usefull extension include:
- *code_prettify* backed by *autopep8* is great for reformatting code in notebook code cells base on PEP 8 style guide
```
pip install autopep8
jupyter nbextension enable code_prettify/autopep8
```
- *spellchecker* highlights incorrectly spelled words in Markdown cells which saved me from a few embarrassing typos.
```
jupyter nbextension enable spellchecker/main
```
- *toggle_all_line_numbers* as its names suggests it adds a toolbar button to toggle between showing line numbers or not
```
jupyter nbextension enable toggle_all_line_numbers/main
```
varInspector is great for debugging python and R kernels. It displays the values of all defined variables in a floating window
jupyter nbextension enable varInspector/main

## Themes
dunovank/jupyter-themes has one of the best themes though one may just stick with the stock theme.
```
pip install jupyterthemes
# dark
jt -t onedork -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T
# light
jt -t grade3 -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T
# Restore default theme
jt -r
```

## Jupyter lab extensions
Here are two pretty useful extentions

- krassowski/jupyterlab-go-to-definition which allow me to use Alt + click to jump to a definition using your mouse, or Ctrl + Alt + B keyboard-only alternative.
```
jupyter labextension install @krassowski/jupyterlab_go_to_definition
```

- krassowski/jupyterlab-lsp adds support for code navigation + hover suggestions + linters + autocompletion. Check out their full list of supported language servers

```
pip install --pre jupyter-lsp
jupyter labextension install @krassowski/jupyterlab-lsp

conda install -c conda-forge python-language-server
```
- *jupyterlab_code_formatter* automates the formatting of code to various standards.

Finally you need to rebuild the jupyter lab app; however, this is done in the Dockerfile where these extensions have been incorporated into the container build.
```
jupyter lab build
```

## Additional Jupyter Lab extension

https://neptune.ai/blog/jupyterlab-extensions-for-machine-learning

## Themes
There are many themes out there, the first customization plugin in my list is not a theme though. It is a topbar extension to quickly switch between light and dark themes

```
jupyter labextension install jupyterlab-topbar-extension jupyterlab-theme-toggle
```

Here is a list a few themes I used recently
```
jupyter labextension install @telamonian/theme-darcula
jupyter labextension install @rahlir/theme-gruvbox
jupyter labextension install @kenshohara/theme-nord-extension
```

--------

