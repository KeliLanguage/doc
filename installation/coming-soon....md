# How to install Keli?

## 1. Download Haskell Stack

{% embed url="https://docs.haskellstack.org/en/stable/install\_and\_upgrade/" %}



## 2. Download Node.js

As long as the version is higher than v10.0.0, it will be fine.

{% embed url="https://nodejs.org/en/download/" %}

## 3. Download Git

{% embed url="https://git-scm.com/downloads" %}



## 4. Build the Keli compiler from source

Open Command Prompt \(for Windows\), or Shell \(for Linux/MacOS\), then paste in the following command.

```bash
mkdir keli
cd keli
git clone https://github.com/KeliLanguage/compiler.git
cd compiler
stack build --copy-bins
```

## 5. Download Visual Studio Code 

{% embed url="https://code.visualstudio.com/download" %}

## 6. Install the Keli extension for Visual Studio Code

{% embed url="https://marketplace.visualstudio.com/items?itemName=wongjiahau.keli" %}





