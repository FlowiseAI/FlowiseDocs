# Variables

Flowise allow users to create variables that can be used in Custom Tool Function.&#x20;

For example, you have a database URL that you do not want it to be exposed on the function, but you still want the function to be able to read the URL from your environment variable.

User can create a variable and get the variable in Custom Tool Function:

`$vars.<variable-name>`

Variables can be Static or Runtime.

### Static

Static variable will be saved with the value specified, and retrieved as it is.

<figure><img src="../.gitbook/assets/image (13).png" alt="" width="542"><figcaption></figcaption></figure>

### Runtime

Value of the variable will be fetched from **.env** file using `process.env`

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt="" width="537"><figcaption></figcaption></figure>



### Resources

* [Pass Variables to Function](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)
