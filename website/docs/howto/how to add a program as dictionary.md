## Add external programs or scripts

GoldenDict can use programs or scripts as dictionary.

### An example

Tokenize the input german sentence into words.

#### Install Python and SoMaJo.

You can follow the instructions in the documentation https://github.com/tsproisl/SoMaJo?tab=readme-ov-file#installation

```
pip install -U SoMaJo
```

#### Create a script

```
from somajo import SoMaJo
import sys

tokenizer = SoMaJo("de_CMC", split_camel_case=True)

# note that paragraphs are allowed to contain newlines
paragraphs = [sys.argv[1]]
sentences = tokenizer.tokenize_text(paragraphs)
for sentence in sentences:
    for token in sentence:
        if token.token_class=='regular':
            print(f"{token.text}")
```

and save the script in the `E:\test.py` for example.

#### Add the script to the program dictionary

![Add the script as program dictionary](add-program-dictionary.png)
`python e:\test.py %GDWORD%`

#### The result

![](program-result.png)

## Connect language-learning tools

The [`gd-tools` project](https://github.com/accordion1650/gd-tools) contains command-line tools that GoldenDict-ng can run as program dictionaries. Install the tools and their dependencies first, then add each command under **Edit → Dictionaries → Programs**. Select **HTML** when the program returns HTML; its output must be written to standard output.

| Use | Command line | Program type |
|-----|--------------|--------------|
| Make Japanese words in a sentence clickable with MeCab | `gd-mecab --word %GDWORD% --sentence %GDSEARCH%` | HTML |
| Segment a Mandarin sentence | `gd-mandarin --word %GDWORD% --sentence %GDSEARCH%` | HTML |
| Translate the current sentence with Argos Translate | `gd-translate --sentence %GDSEARCH%` | HTML |

The MeCab tools need their language dictionaries installed. The Mandarin dictionary is an optional download described in the [`gd-tools` README](https://github.com/accordion1650/gd-tools#gd-mandarin); without it, `gd-mandarin` displays setup guidance instead of parsing text. `gd-translate` also needs Argos Translate and a language package, as described in the [translation section](https://github.com/accordion1650/gd-tools#gd-translate).

For a word-level Google Translate example, the [`google-translate-for-goldendict` repository](https://github.com/accordion1650/google-translate-for-goldendict) documents this command:

```text
python3 -m googletranslate zh-CN %GDWORD%
```

Set its program type to **HTML** or **Text**, matching the selected output format. This helper depends on the Python package and Google Translate service described in its README.

Use `%GDWORD%` when a tool expects one word and `%GDSEARCH%` when it needs the full search-box text. GoldenDict-ng replaces each placeholder as a separate argument, so sentence spaces are preserved. If GoldenDict-ng cannot find a command on macOS or Linux, enter the executable's full path in the command line.



