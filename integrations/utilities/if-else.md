# If Else

Flowise allows you to split your chatflow into different branches depending on If/Else condition.

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

### Input Variables

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As noticed from the image above, it takes in any nodes that has `json` output. Some examples are: Custom Function, LLM Chain Output Prediction, Get/Set Variables.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

You can then give a variable name:

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

This variable can then be used in the [If Function](if-else.md#if-function) and [Else Function](if-else.md#else-function) with the prefix `$`. For example:

```
$output
```

### If Else Name

You can name the node for easier visualization of what it does.

### If Function

This is a piece of JS code that is ran on Node sandbox. It must:

* Contains the `if` statement
* Returns a value within `if` statement

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="312"><figcaption></figcaption></figure>

This gives much more flexibility for users to do complex comparison like regex, date comparsion and many more.

### Else Function

Similar to If Function, it must returns a value. This function will only be ran if the [If Function](if-else.md#if-function) does not return a value.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="317"><figcaption></figcaption></figure>

### Output

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

When the [If Function](if-else.md#if-function) successfully returns a value, it will be passed to the **True** output dot as shown above. This allow users to pass the value to the next node.

Otherwise, the returned value from [Else Function](if-else.md#else-function) will be passed to the **False** output dot.

User can also take a look at the If Else template in the marketplace:

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>
