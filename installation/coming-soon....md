# How to install Keli?

## 1. Download Haskell Stack

{% embed url="https://docs.haskellstack.org/en/stable/install\_and\_upgrade/" %}



## 2. Download Node.js

{% embed url="https://nodejs.org/en/download/" %}

## 3. Build the Keli compiler from source

```bash
mkdir keli
cd keli
git clone https://github.com/KeliLanguage/compiler.git
cd compiler
stack build --copy-bins
```

## 4. Install the Keli extension for Visual Studio Code

{% embed url="https://marketplace.visualstudio.com/items?itemName=wongjiahau.keli" %}





