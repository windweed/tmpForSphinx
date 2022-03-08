# Erlang in IntelliJ IDEA

## 1 Start with an Erlang project

### 1.1 Set up the environment

**Install and enable Erlang plugin**

1. **Settings/Preferences (Ctrl+Alt+S)**, select **Plugins** on the left.
2. Search **Erlang**, Install, restart the IDE.

**Install Erlang/OTP framework**

Once the installation is over, add `<Erlang installation folder>\bin` to `PATH`.

**Install Rebar3**

TODO

**Configure SDK**

### 1.2 Create a new Erlang project

You can create a new Erlang project in IntelliJ IDEA in one of two ways:
* Use the **New Project** wizard to create a bare-bones project with minimal structure.
* Generate the project's structure from a Rebar3 template using the **Terminal** tool window.

**Use the New Project wizard**

1. From the main menu, select **File -> New -> Project**.
    In the dialog that appears, choose **Erlang** as the project's base.
    Ignore the additional frameworks at this point and click **Next**.
2. In the dialog that appears, choose **Erlang** as the projct's base.
    Ignore additional frameworks at this point and click **Next**.
3. In this step, **Erlang SDK** will be pre-selected as the project's SDK.
    If you have several versions of the SDK installed, choose the one that
    you need from the list, then click **Next**.
4. Give your project a name and choose where to save it. Click **Finish** to
    close the dialog.



