
### StringUtils.hasText

StringUtils.hasText() 是 Spring Framework 中的一个静态方法，用于判断字符串是否为空或者空格。

具体来说，该方法会先使用 `StringUtils.isNotBlank()` 方法判断**字符串是否为空**，如果不为空，再使用 `StringUtils.containsNone` 方法判断**是否包含空格**。

如果字符串为 **null**、**空字符串**、**仅由空格组成的字符串**，那么该方法都会**返回false**，否则返回 true。

```java
StringUtils.hasText(null); // false
StringUtils.hasText(""); // false
StringUtils.hasText(" "); // false
StringUtils.hasText("hello"); // true
StringUtils.hasText("hello world"); // true
```