# Digital Credentials: Federated Identity on the Web Working Group

This repository is being used for work in the W3C Federated Identity on the Web Working Group, governed by the [W3C Patent Policy](http://www.w3.org/Consortium/Patent-Policy-20040205/) and
[Software and Document License](https://www.w3.org/copyright/software-license-2023/). To contribute, you must 
either participate in the relevant W3C Working Group or make a [non-member patent licensing
 commitment](https://www.w3.org/policies/process/#contributor-license).


If you are not the sole contributor to a contribution (pull request), please identify all
contributors in the pull request comment.

To add a contributor (other than yourself, that's automatic), mark them one per line as follows:

```
+@github_username
```

If you added a contributor by mistake, you can remove them in a comment with:

```
-@github_username
```

If you are making a pull request on behalf of someone else but you had no part in designing the
feature, you can remove yourself with the above syntax.

## Spell Checking

This repository uses [cspell](https://cspell.org/) for spell checking. Spelling is automatically checked on pull requests via GitHub Actions.

### Using cspell with Visual Studio Code

To enable spell checking in VS Code:

1. Install the [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) extension by streetsidesoftware.
2. The extension will automatically use the `cspell.json` configuration file in this repository.
3. Spelling errors will be highlighted with a blue squiggly underline.
4. To add a word to the project dictionary, use the Quick Fix menu (Ctrl+. or Cmd+.) and select "Add word to cspell.json".

Custom dictionaries for this project are located in the `.cspell/` directory.
