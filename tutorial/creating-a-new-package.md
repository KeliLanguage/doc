# Creating a new package

Let's say we want to create a new Keli package named `MyPackage` , we can achieve this by typing the following command:

```text
keli new-package MyPackage
```

After typing this command, you should see some output message like this:

```text
Creating package `MyPackage`
Creating _src folder
Initializing purse.json
Creating _test folder
Creating README file
Creating LICENSE file
Creating .gitignore file
Initialized empty Git repository in /home/hou32hou/Repos/MyPackage/.git/

Staging and commiting initial files

==== Package successfully created! ====

Type the following command to go into your package:

  cd MyPackage

```

Then, type the following command to go into the `MyPackage` directory:

```text
cd MyPackage
```

After that, we need to add `corelib` as dependencies, we can do that by typing:

```text
keli add-dependency https://github.com/KeliLanguage/corelib.git 0.0.1-e
```

Now, open the folders of `MyPackage` in Visual Studio Code by typing:

```text
code .
```

When you open the file trees in VSCode, you will notice that there are the `_src` folder, this is the folder where you will store your Keli source files for our package `MyPackage` .

You might have notice that there is nothing inside the `_src` folder. To try things out, let us create a file named `MyPackageDemo.keli` inside the `_src` folder.

Then, paste in the following contents into `MyPackageDemo.keli` :

```c
= module.import("../KeliLanguage.corelib.0.0.1-e/Boolean.keli")
= module.import("../KeliLanguage.corelib.0.0.1-e/Math.keli")
= module.import("../KeliLanguage.corelib.0.0.1-e/String.keli")
= module.import("../KeliLanguage.corelib.0.0.1-e/List.keli")

= "Hello world".replace("world") with("Keli!")

= 123.+(456).*(789)

= [1,2,3,4,5].toList.select(.*(2))

= [1,2,3,4,5].toList.where(.>(3))
```

Now, open `MyPackageDemo.keli` in VSCode. 

Then, click on the `Run this Keli Program` button located at the bottom of VSCode \(note that this will only be true if you already install the [Keli VSCode Extension](../installation/instructions.md#6-install-the-keli-extension-for-visual-studio-code)\). 

After that, you should see the following:

![Output of each expression is shown after pressing \[Run this Keli program\]](../.gitbook/assets/image%20%284%29.png)

That's all. Thank you for reading. Enjoy!

