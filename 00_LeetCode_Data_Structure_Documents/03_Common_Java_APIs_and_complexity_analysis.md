# Common Java APIs and complexity analysis

## 1 String Common Methods

### 1.1 String’s Common Methods

```java
//import java.util.Scanner;
Scanner scanner = new Scanner(System.in);
if (scanner.hasNext()) {
    String currentLineString = scanner.next();
}
//Java's "+" is essentially StringBuilder.append()
String addedString = 1 + 2 + "test" + 1 + 2 + 'a' + 3.4; //The result is "3test12a3.4"
//StringBuffer().append() appends the string representation of the argument to this sequence.
StringBuffer appendedStringBuffer = new StringBuffer().append(1 + 2).append("test").append(1).append(2).append('a').append(3.4);  //The result is also "3test12a3.4"

//String().split(String) splits this string around matches of the given regular expression.
String[] splittedStringArray = "how-r-u".split("-"); //The result is ["how", "r", "u"]
String[] splittedStringArray2 = "how-r-u".split(":"); //The result is ["how-r-u"]
//String.join() returns a new String composed of the CharSequence elements joined together with the specified delimiter.
String joinedString = String.join(",", Arrays.asList("a", "bb", "cd")); //The result is "a,bb,cd"
String joinedString2 = String.join(",", new String[]{"a", "b"}); //The result is "a,b"
//StringJoiner is used to construct a sequence of characters separated by a delimiter and optionally starting with a supplied prefix and ending with a supplied suffix.
StringJoiner stringJoiner = new StringJoiner(",", "Prefix:", ":Suffix");
String outputtedStringJoiner = stringJoiner.add("a").add("b").add("c").toString(); //The result is "Prefix:a,b,c:Suffix"

//String().contains() returns true if and only if this string contains the specified sequence of char values.
boolean ifContains = "abcd".contains("ab"); //The result is true
//String().startWith() tests if this string starts with the specified prefix.
boolean isStartWithCat = "CatVSDog".startsWith("Cat"); //The result is true
//String().endsWith() tests if this string ends with the specified suffix.
boolean isEndWithCat = "CatVSDog".endsWith("Dog"); //The result is true
//String().equals() compares this string to the specified string.
boolean isEqual = "abc".equals("abc"); //The result is true
//String().equalsIgnoreCase() compares this string to the specified string ignoring case.
boolean isEqualIgnoringCase = "abc".equalsIgnoreCase("aBc"); //The result is true
//String().substring() returns a string that is a substring of this string.
String substring1ToEnd = "abcd".substring(1); //The result is "bcd"
String substring1To3 = "abcd".substring(1,3); //The result is "bc"
//String().toLowerCase() converts this String to lower case
String stringToLowerCase = "abCD".toLowerCase(); //The result is "abcd"
//String().toUpperCase() converts this String to upper case
String stringToUpperCase = "abCD".toUpperCase(); //The result is "ABCD"
//String().indexOf() returns the index within this string of the first occurrence of the specified substring, or -1 is returned.
int firstIndexOfString = "abcabcab".indexOf("ab"); //The result is 0
//String().lastIndexOf() returns the index within this string of the last occurrence of the specified substring, or -1 is returned.
int lastIndexOfString = "abcabcab".lastIndexOf("ab"); //The result is 6，prints the first character subscript of the last substring "ab"
//Get the first character subscript of the first matched substring from the i-th to the last element of a string
//String().indexOf() returns the index within this string of the first occurrence of the specified substring, starting at the specified index.
int firstIndexOfSubstring = "abcabcab".indexOf("ab",1);//Prints the subscript 3 of the first character of the second substring "ab"
//Get the first character subscript of the last substring from zeroth to i-th characters of a string
//String().lastIndexOf() returns the index within this string of the last occurrence of the specified substring, searching backward starting at the specified index.
int lastIndexOfSubstring = "abcabcab".lastIndexOf("ab",5); //The result is 3

//String().compareTo() compares two strings lexicographically. The comparison is based on the Unicode value of each character in the strings.
//(1)If two strings start with different letters, returns the difference of the ASCII Code for the first letter
//The result is a positive integer if this String object lexicographically follows the argument string.
int positiveCompareToString = "cde".compareTo("ab"); //The result is 2
//The result is a negative integer if this String object lexicographically precedes the argument string.
//(2)If two strings have same prefix, the ASCII Code difference of the different characters is returned until there is a different next pair of characters
int negativeCompareToString = "aab".compareTo("acde"); //The result is -2
//(3)Returns the length difference between two strings if they have same prefixs
int negativeCompareToString2 = "ab".compareTo("abcde"); //The result is -3
//(4)The inputted String can't be null, or throws a NullPointerException.
int negativeCompareToString3 = "ab".compareTo(null); //The expression throws a NullPointerException.
//The result is zero if the strings are equal, returns 0 exactly when the equals(Object) method would return true.
int zeroCompareToString = "ab".compareTo("ab"); //The result is 0
```

### 1.2 StringBuffer and StringBuilder

```java
//StringBuffer has all the same methods as StringBuilder, but StringBuffer is thread-safe.
StringBuilder stringBuilder = new StringBuilder();
//StringBuilder().append() appends the inputted character(s) to its end.
stringBuilder.append(new StringBuffer("abc")).append('1').append(2.3); // The result is "abc12.3"
//StringBuilder().deleteCharAt() removes the character at the specified position in this sequence.
stringBuilder.deleteCharAt(1); //"abc12.3" -> "ac12.3"
//StringBuilder.delete() removes the characters ranging in the string from the inclusive start to the exclusive end.
stringBuilder.delete(1,2); //"ac12.3" -> "a12.3"
//StringBuilder.insert() inserts inputted character(s) into the sequence at the position indicated by offset.
stringBuilder.insert(1, 'd'); //"a12.3" -> "ad12.3"
stringBuilder.reverse(); // "ad12.3" -> "3.21da"
//StringBuilder.setCharAt() replaces the character at the specified index with the inputted character.
stringBuilder.setCharAt(1, 'e'); //"3.21da" -> "3e21da"
//StringBuilder.replace() replaces the characters in a substring of this sequence with characters in the inputted String.
stringBuilder.replace(1, 3, "xyz"); //"3e21da" -> "3xyz1da"
//StringBuilder.toString() transfer itself to String.
String transferToString = stringBuilder.toString();

int compareResult = stringBuilder.compareTo(new StringBuilder("ab"));
int firstIndexOfSubstring = stringBuilder.indexOf("a");
int lastIndexOfSubstring = stringBuilder.lastIndexOf("a");
int stringBuilderLength = stringBuilder.length();
char charAtStringBuilder = stringBuilder.charAt(2);
String substring = stringBuilder.substring(2);
```

### 1.3 String’s Mutual Type Conversion

```java
//String.valueOf() returns the string representation of the inputted argument.
String integerString = String.valueOf(123); //The result is "123"
String decimalString = String.valueOf(12.34); //The result is "12.34"
String booleanString = String.valueOf(true); //The result is "true"
String characterString = String.valueOf('a'); //The result is "a"
//new String(char[] charArray) allocates a new String so that it represents the characters array.
String characterArrayString = new String(new char[]{'a', 'b', 'c'}); //The result is "abc"
//String().charAt() returns the char value at the specified index. An index ranges from 0 to length() - 1.
char characterAtAString = "string".charAt(2); //The result is 'r'
//String().toCharArray() converts this string to a new character array.
char[] characterArray = "string".toCharArray(); //The result is [s, t, r, i, n, g]
//Integer.parseInt() parses the string argument as a signed decimal integer, or throw a NumberFormatException.
int parsedInteger = Integer.parseInt("123"); //The result is 123
boolean parsedBoolean = Boolean.parseBoolean("true"); //The result is true
//"toPlainString()" returns a string representation of this BigDecimal without an exponent field.
String bigIntegerAndBigDecimalString = new BigInteger("1").toString() + new BigDecimal("0.1").toString() + new BigDecimal("0.1").toPlainString();
```

### 1.4 String’s Regex Methods

```java
//String().replace() replaces each substring of this string that matches the target sequence with the replacement sequence. 
String replacedString = "abbbc".replace("bb","o"); //The result is "aobc"
//String().replaceFirst() replaces the first substring of this string that matches the target sequence with the replacement sequence. 
String firstReplacedString = "abbbbc".replaceFirst("bb","o"); //The result is "aooc"
//"(.)"represents a capture group that matches any character, "\\1" represents what is captured in the first capture group, "\\1*" represents what is captured in the first capture group can be duplicated by 0~n times, so "(.) \\1\\1*" is used to match strings with two or more same character, such as "bb" or "bbbbb". When the regular expression matches multiple results, which preferentially selects the longest match result "bbbbb". "$1" represents what is captured by the first capture group, which is the string "(.)" matched, such as "b".
String replacedAllString = "abbbbbc".replaceAll("(.)\\1\\1*","$1$1"); //The result is "abbc"
//Pattern.matches() uses the regular expression to match the given string.
boolean isRegexMatched = Pattern.matches("^\\w{2}$", "ab"); //The result is true
```

### 1.5 Remove leading and trailing space去除空白字符

```java
//String().trim() returns a string whose value is this string, with all leading and trailing space removed, where space is defined as any character whose codepoint is less than or equal to 'U+0020' (the space character).
//A code point, which is a Unique integer assigned to each character.
String trimmedSentence = " Hi, my friend!  ".trim(); //The value of "trimmedSentence" is "Hi, my friend!"
//Since Java11, String().strip() returns a string whose value is this string, with all leading and trailing Character#isWhitespace(int) white space removed.
String stripedSentence = " How are you?  ".strip(); //The value of "stripedSentence" is "How are you?"
System.out.println("".trim());
System.out.println("".strip());
//String().isEmpty() returns true if and only if, the string's length is 0.
boolean isStringEmpty = "".isEmpty(); //The value of "isStringEmpty" is "true"
//String().isBlank() returns true if the string is empty or contains only Character#isWhitespace(int) white space codepoints, otherwise false.
boolean isStringBlank = " ".isBlank(); //The value of "isStringBlank" is "true"
//String().replace(CharSequence,CharSequence) replaces each substring of this string that matches the literal target sequence with the specified literal replacement sequence.
//The value of "stringAfterReplaceBlankAsEmpty" is "Howareyou?" //The value of "stringAfterReplaceBlankAsEmpty" is "Howareyou?"
String stringAfterReplaceBlankAsEmpty = " How are you?  ".replace(" ", "");

//String().replaceAll(String,String) replaces each substring of this string that matches the given regular expression with the given replacement.
//The value of "stringAfterReplaceAllBlankAsEmpty" is "Howru", "\\s" stands for a blank, "+" stands for one or more
String stringAfterReplaceAllBlankAsEmpty = " How r u ".replaceAll("\\s+", "");
```

## 2 Math, Operator and Big Numbers

### 2.1 Java Binary Operator `/` and`%`

#### 2.1.1 Java Binary Operator `/`

According to [Oracle Java SE Specifications -  15.17.2. Division Operator /](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.oracle.com%2Fjavase%2Fspecs%2Fjls%2Fse13%2Fhtml%2Fjls-15.html%23jls-15.17.2). The binary operator `/` performs division, producing the quotient of its operands. The left-hand operand is the *dividend* and the right-hand operand is the *divisor*.

The quotient produced by the divisible symbol `/` is the result of normal division followed by subtracting the decimal part, rather than the result of rounding. The positive and negative sign of the quotient is determined by both signs of the dividend and the divisor.

整除符号`/`产生的最终数值是普通除法后再去除小数位的数值，而不是四舍五入的结果。整除符号`/`产生的最终数值的正负符号由除数和被除数共同决定。

```java
System.out.println(4 / 3);   //Output 1
System.out.println(5 / 3);   //Output 1
System.out.println(-4 / 3);  //Output -1
System.out.println(-5 / 3);  //Output -1
System.out.println(4 / -3);  //Output -1
System.out.println(5 / -3);  //Output -1
System.out.println(-4 / -3); //Output 1
System.out.println(-5 / -3); //Output 1
```

> lexicographically /leksɪkəˈɡræfɪkəli/  adv按照字典序地	inputted /ɪnˈpʊtɪd/
>
> magnitude /ˈmæɡnɪtuːd/ n量级,程度(在下文中可以近似理解为绝对值)
>
> **Absolute value**, Measure of the magnitude of a [real number](https://www.britannica.com/science/real-number), [complex number](https://www.britannica.com/science/complex-number), or [vector](https://www.britannica.com/science/vector-mathematics).

There is one special case that does not satisfy this rule: if the dividend is the negative integer of largest possible magnitude for its type, and the divisor is `-1`, then integer overflow occurs and the result is equal to the dividend. Despite the overflow, no exception is thrown in this case.

```java
System.out.println(Integer.MIN_VALUE/-1); //Output -2147483648
```

#### 2.1.2 Java Binary Operator `%`

According to [Oracle Java SE Specifications -  15.17.3. Remainder Operator `%`](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.oracle.com%2Fjavase%2Fspecs%2Fjls%2Fse13%2Fhtml%2Fjls-15.html%23jls-15.17.3). The binary `%` operator is said to yield the remainder of its operands from an implied division; the left-hand operand is the *dividend* and the right-hand operand is the *divisor*.

It follows from this rule that the result of the remainder operation can be negative only if the dividend is negative, and can be positive only if the dividend is positive.

模运算符号`%`产生的最终数值是被模数和模数的绝对值取模后再复用模数正负号的数值，即其运算结果的符号始终和被模数的符号一致。

```java
System.out.println(4 % 3); //Output 1
System.out.println(5 % 3); //Output 2
System.out.println(-4 % 3); //Output -1
System.out.println(-5 % 3); //Output -2
System.out.println(4 % -3); //Output 1
System.out.println(5 % -3); //Output 2
System.out.println(-4 % -3); //Output -1
System.out.println(-5 % -3); //Output -2
```

### 2.2 Java Array, String and Collection’s Length

1. `.length` is for getting the length of an array, such as `int arrayLength = new int[2].length;`.
2. `.length()` is  for getting the length of a string, such as `int stringLength = "string".length();`.
3. `.size()` is for getting the length of a collection, such as `int collectionLength = new ArrayList<>(Arrays.asList(1, 2)).size();`. `int size();` is one of Java `Collection` Interface’s interface methods.

```java
int arrayLength = (new int[]{1, 2, 3}).length; //The result is 3
String[][][] multiDimensionalArray = new String[4][5][6];
int firstDimensionArrayLength = multiDimensionalArray.length; //The result is 4
int secondDimensionArrayLength = multiDimensionalArray[0].length; //The result is 5
int thirdDimensionArrayLength = multiDimensionalArray[0][0].length; //The result is 6
int stringLength = "string".length(); //The result is 6
int collectionLength = new ArrayList<>(Arrays.asList(1, 2)).size(); //The result is 2
```

### 2.3 Math Utility

```java
//向正无穷取整的最小double型整数，Returns the smallest double-type integer that is greater than or equal to the argument
double ceilingDoubleValue = Math.ceil(-1.99);//The result is "-1.0"
//向负无穷下取整的最大double型整数，Returns the largest double-type integer that is less than or equal to the argument
double flooredDoubleValue = Math.floor(-1.01);//The result is "-2.0"
//向最接近的整数四舍五入，若小数部分正好为0.5则趋向正无穷，如1.5->2，-1.5->-1，Returns the closest long to the argument, with ties rounding to positive infinity.
long roundedPositiveValue = Math.round(1.49) + Math.round(1.5) + Math.round(1.51);//1+2+2=5
long roundedNegativeValue = Math.round(-1.49) + Math.round(-1.5) + Math.round(-1.51);//-1+-1+-2=-4
System.out.println(roundedPositiveValue + " "+roundedNegativeValue);
double poweredValue = Math.pow(1.2, 3.4);//(1.2)^(3.4)=1.858729691979481
//Returns the correctly rounded positive square root of a double value.
double squareRoot = Math.sqrt(9.0);//The result is "3.0"
//Returns the absolute value of a double value.
double absoluteValue = Math.abs(-1.5);//The result is "1.5"
double minAndMaxValue = Math.min(1, 2) + Math.max(2.5, 4.5);//1+4.5=5.5
//Returns a double value with a positive sign, greater than or equal to 0.0 and less than 1.0
double randomDecimalValue = Math.random();
//Returns the next pseudorandom, uniformly distributed int value from all 2^32 possible int values with equal probability.
new Random().nextInt();
//Returns a pseudorandom, uniformly distributed int value between 0 (inclusive) and the specified value (exclusive)
new Random().nextInt(10);
//Returns the next pseudorandom, uniformly distributed double value between 0.0 and 1.0 from this random number generator's sequence.
new Random().nextDouble();
```

### 2.4 BigInteger

```java
//BigInteger stores all digits using an array of ints.
//Returns a BigInteger whose value is equal to the inputted long number.
BigInteger bigInteger = BigInteger.valueOf(-12);
//Translates the decimal String into a BigInteger. The String representation consists of an optional minus sign followed by one or more decimal digits.
bigInteger = new BigInteger("-000012");//"-000012" is legal, representing "-12"
//Translates a BigInteger into primitive types.
double bigIntegerValues = bigInteger.floatValue() + bigInteger.doubleValue() + bigInteger.byteValueExact() + bigInteger.intValueExact() + bigInteger.longValueExact() + bigInteger.shortValueExact();
//Converts this BigInteger to a double, if this BigInteger has too great a magnitude to represent as a double, it will be converted to Double. NEGATIVE_INFINITY or Double. POSITIVE_INFINITY as appropriate.
double doubleConvertedByBigInteger =  BigInteger.valueOf(9999999999L).doubleValue();
//Converts the BigInteger to an int, if the BigInteger is out of the int range, only the low-order 32 bits are returned.
int intConvertedByBigInteger = BigInteger.valueOf(Integer.MAX_VALUE + 1L).intValue();//The result is "-2147483648"
try {
    //Converts the BigInteger to an int, if the BigInteger is out of the int range, then an ArithmeticException is thrown.
    int intExactConvertedByBigInteger = BigInteger.valueOf(Integer.MAX_VALUE + 1L).intValueExact();
    //NumberFormatException: For input string: "123.4a5"
    new BigInteger("00123.4a5");
} catch (Exception ignored) {
}
//bigInteger's value is -12. BigInteger.compareTo() returns -1, 0 or 1 as this BigInteger is numerically less than, equal to, or greater than the inputted value.
int comparedResult = bigInteger.compareTo(new BigInteger("0"));//The result is "-1"
BigInteger addedBigInteger = bigInteger.add(new BigInteger(String.valueOf(10)));//The result is "-2"
BigInteger subtractedBigInteger = bigInteger.subtract(new BigInteger("1"));//The result is "-13"
BigInteger multipliedBigInteger = bigInteger.multiply(new BigInteger("2"));//The result is "-24"
BigInteger dividedBigInteger = bigInteger.divide(new BigInteger("5"));//The result is "-2", the remainder is ignored
BigInteger remainedBigInteger = bigInteger.remainder(new BigInteger("7"));//The result is "-5", which is equivalent to "%"
BigInteger absoluteBigInteger = bigInteger.abs();//The result is "12"
BigInteger exponentialBigInteger = bigInteger.pow(2);//The result is "144"
BigInteger squareRootedBigInteger = new BigInteger("16").sqrt();//The result is "4"
```

### 2.5 BigDecimal

```java
//A BigDecimal is represented by a BigInteger, which represents a complete integer, and a scale, which represents the number of decimal places.
String bigDecimalString = new BigDecimal(0.125).toString();//The result is 0.125
//Returns a string representation of this BigDecimal without an exponent field.
String bigDecimalPlainString = new BigDecimal(0.1).toPlainString();//The result is 0.1000000000000000055511151231257827021181583404541015625
//Initial a BigDecimal by String is better than Number, the latter may cause precision loss.
BigDecimal bigDecimal = new BigDecimal("0.125");//The value is 0.125 without precision loss
//If zero or positive, the scale is the number of digits to the right of the decimal point. If negative, the unscaled value of the number is multiplied by ten to the power of the negation of the scale.
int scaleSum = new BigDecimal("-0.001").scale() + new BigDecimal("1200.0100").scale();//3+4=7
//Returns a BigDecimal which is numerically equal to this one but with any trailing zeros removed from the representation
int positiveScale = new BigDecimal("12000.0100").stripTrailingZeros().scale();//The final scale is 2
int negativeScale = new BigDecimal("12000.0000").stripTrailingZeros().scale();//The final scale is -3
//Returns a BigDecimal whose scale is the specified value, and whose value is numerically equal to this BigDecimal's. Throws an ArithmeticException if this is not possible.
new BigDecimal("1234.5678").setScale(2);//ArithmeticException: Rounding necessary
new BigDecimal("1234.5678").setScale(2, RoundingMode.HALF_EVEN);//The result is 1234.57
new BigDecimal("1234.5678").setScale(-2, RoundingMode.CEILING);//The result is "1.3E+3", which is 1300
new BigDecimal("1234.5678").setScale(6);//The result is 1234.567800
/**
 * Each rounding mode indicates how the least significant returned digit of a rounded result is to be calculated.
 * Input    UP  DOWN    CEILING FLOOR   HALF_UP HALF_DOWN   HALF_EVEN   UNNECESSARY
 * 5.5     6  5      6      5      6      5          6          ArithmeticException
 * 2.5     3  2      3      2      3      2          2          ArithmeticException
 * 1.6     2  1      2      1      2      2          2          ArithmeticException
 * 1.1     2  1      2      1      1      1          1          ArithmeticException
 * 1.0     1  1      1      1      1      1          1          1
 * -1.0        -1 -1     -1     -1     -1     -1         -1         -1
 * -1.1        -2 -1     -1     -2     -1     -1         -1         ArithmeticException
 * -1.6        -2 -1     -1     -2     -2     -2         -2         ArithmeticException
 * -2.5        -3 -2     -2     -3     -3     -2         -2         ArithmeticException
 * -5.5        -6 -5     -5     -6     -6     -5         -6         ArithmeticException
 */
//Returns a BigDecimal whose value is (this + augend), and whose scale is max(this.scale(), augend.scale()). augend /ˈɔːdʒend/ 被加数
BigDecimal addedBigDecimal = new BigDecimal("1.23").add(new BigDecimal("4.5670000"));//The result is 5.7970000
//Returns a BigDecimal whose value is (this - subtrahend), and whose scale is max(this.scale(), subtrahend.scale()).
BigDecimal subtractedBigDecimal = new BigDecimal("1.23").subtract(new BigDecimal("4.5670000"));//The result is -3.3370000
//Returns a BigDecimal whose value is (this × multiplicand), and whose scale is (this.scale() + multiplicand.scale()).
BigDecimal multipliedBigDecimal = new BigDecimal("1.23").multiply(new BigDecimal("2.0000"));//The result is 2.460000
//Returns a BigDecimal whose value is the integer part of the quotient (this / divisor) rounded down.
BigDecimal dividedIntegralBigDecimal = new BigDecimal("35.99").divideToIntegralValue(new BigDecimal("2.00000"));//The result is 17
//Returns a BigDecimal whose value is (this % divisor).
BigDecimal dividedBigDecimalRemainder = new BigDecimal("35.99").remainder(new BigDecimal("2"));//The result is 1.99
BigDecimal dividedBigDecimal2 = new BigDecimal("2.46").divide(new BigDecimal("2.0000"), 1, RoundingMode.HALF_EVEN);//The result is 1.2

//Unlike "compareTo", this method considers two BigDecimal objects equal only if they are equal in value and scale, thus 2.0 is not equal to 2.00
boolean isBigDecimalEqual = new BigDecimal("1.2").equals(new BigDecimal("1.20"));//The result is false
boolean isBigDecimalEqual2 = new BigDecimal("1.2").equals(new BigDecimal("1.2"));//The result is true
boolean isBigDecimalEqual3 = new BigDecimal("1.2").equals(new BigDecimal("1.20").stripTrailingZeros());//The result is true
//Equal in value but have a different scale (like 2.0 and 2.00) are considered equal. Returns -1, 0, or 1 as this BigDecimal is numerically less than, equal to, or greater than the inputted value.
int isBigDecimalSameValue = new BigDecimal("1.2").compareTo(new BigDecimal("1.20"));//The result is 0
int isBigDecimalBigger = new BigDecimal("1.3").compareTo(new BigDecimal("1.20"));//The result is 1

BigDecimal poweredBigDecimal = new BigDecimal(-2).pow(3);//The result is -8
BigDecimal absoluteBigDecimal = new BigDecimal(-2).abs();//The result is 2
//Converts to an int. Any fractional part will be discarded, and if it is too big to fit in an int, only the low-order 32 bits are returned.
int intValue = new BigDecimal("12.3").intValue();//The result is 12
//Converts to an int. If it has a nonzero fractional part or is out of the possible range for an int result then throw an ArithmeticException.
int intValueExact = new BigDecimal("1.2").intValueExact();//ArithmeticException: Rounding necessary
//Converts to a BigInteger. Any fractional part of this BigDecimal will be discarded.
BigInteger convertedBigInteger = new BigDecimal("12.3").toBigInteger();//The result is 12
//Converts to a BigInteger. An exception is thrown if this BigDecimal has a nonzero fractional part.
BigInteger convertedExactBigInteger = new BigDecimal("1.2").toBigIntegerExact();
```

## 3 Collections

### 3.1 List and Set(ArrayList and HashSet)

```java
//Collections(List, Set or Queue) common methods
Collection<Integer> collection = new ArrayList<>();
int collectionSize = collection.size();
//Returns true if this collection changed as a result of the call, returns false if this collection does not permit duplicates and already contains the specified element.
boolean isAddSuccessfully = collection.add(1);//[]->[1], return true as it's ArrayList
//Returns true if this collection changed as a result of the call, vice-versa
boolean isAllAddSuccessfully = collection.addAll(Arrays.asList(1, 1, 2));//[1]->[1,1,1,2], return true
//Removes all specified elements, even if this collection contains one or more such elements, returns true if an element was removed successfully
boolean isRemovedSuccessfully = collection.remove(1);//[1,1,1,2]->[2], return true
collection.addAll(Arrays.asList(1, 2, 2, 4));//[2]->[1,2,2,2,4]
//Removes all specified elements, returns true if at least an element was removed successfully
boolean isAllRemovedSuccessfully = collection.removeAll(Arrays.asList(2, 3));//[1,2,2,2,4]->[1,4], return true
//Returns true if this collection contains at least one specified element.
//Returns true if this collection contains no elements.
boolean isEmpty = collection.isEmpty();
boolean isContain = collection.contains(1);//[1,4] contains 1, return true
//Returns true if this collection contains all the elements in the specified collection.
boolean isAllContain = collection.containsAll(Arrays.asList(1, 2));//[1,4] contains 1 but not 2, return false
boolean isAllContain2 = collection.containsAll(Arrays.asList(1, 4));//[1,4] contains [1,4], return true
//Returns an Object array containing all elements in this collection
Object[] objectArray = collection.toArray();
//Returns an array containing all elements in this collection; the runtime type of the returned array depends on the specified array.
Integer[] intArray = collection.toArray(new Integer[0]);
//Removes all the elements from this collection
collection.clear();

//Several ways to initialize a list
ArrayList<Integer> arrayList = new ArrayList<>();
arrayList.add(1);
arrayList.add(2);
//Transfer a HashSet to an ArrayList
arrayList = new ArrayList<>(new HashSet<>());
//Use Anonymous Classes to initialize a list, the outer braces define the anonymous classes, the inner braces define the Instance Initialization Block.
arrayList = new ArrayList<>() {{
    this.add(1);
    add(2);
}};
arrayList.add(3);
arrayList = new ArrayList<>(Arrays.asList(1, 2, 3));
arrayList.add(4);
arrayList = new ArrayList<>(Collections.nCopies(5, 666));
arrayList.add(5);
//Since Java9, List.of() returns an unmodifiable list
arrayList = new ArrayList<>(List.of(1, 2, 3));
arrayList.add(6);
//Arrays.asList(List) returns a fixed-size immutable list backed by the specified array.
List<Integer> immutableArrayList = Arrays.asList(1, 2, 3);
immutableArrayList.add(4);//Throws an UnsupportedOperationException
//Collections.nCopies(n, object) returns an immutable list consisting of n copies of the specified object.
immutableArrayList = Collections.nCopies(5, 666);
immutableArrayList.add(6);//Throws an UnsupportedOperationException
//Since Java9, List.of() returns an unmodifiable list
immutableArrayList = List.of(1, 2, 3);
immutableArrayList.add(7);//Throws an UnsupportedOperationException

//exclusive List methods
arrayList = new ArrayList<Integer>(Arrays.asList(1, 2, 3));
//Returns the element at the specified index in this list, or throws an IndexOutOfBoundsException
Integer specifiedIndexElement = arrayList.get(-1);//IndexOutOfBoundsException: Index -1 out of bounds
specifiedIndexElement = arrayList.get(2);//[1,2,3]->3
arrayList.add(4);//[1,2,3]->[1,2,3,4]
//Inserts the specified element at the specified index(0 to the list length is legal index), shifts the element(S) at or behind that position to the right, or throws an IndexOutOfBoundsException
arrayList.add(4, 5);//[1,2,3,4]->[1,2,3,4,5];[1,2,3]->index 4 is out of bounds, throws an IndexOutOfBoundsException
arrayList.add(0, 0);//[1,2,3,4,5]->[0,1,2,3,4,5]
//Inserts all specified elements at the specified index(0 to the list length is legal index), shifts the element(S) at or behind that position to the right, or throws an IndexOutOfBoundsException
arrayList.addAll(6, Arrays.asList(6));//[0,1,2,3,4,5]->[0,1,2,3,4,5,6]
//Removes the element at the specified index in this list, shifts any subsequent elements to the left, or throws IndexOutOfBoundsException
arrayList.remove(5);//[0,1,2,3,4,5,6]->[0,1,2,3,4,6]
//Replaces the element at the specified index with the specified element, or throws IndexOutOfBoundsException
arrayList.set(5, 5);//[0,1,2,3,4,6]->[0,1,2,3,4,5]
//Returns the index of the first occurrence of the specified element in this list, or -1 if this list does not contain the element.
int indexOfElement3 = arrayList.indexOf(3);//[0,1,2,3,4,5]->The result is 3
//Returns the index of the last occurrence of the specified element in this list, or -1 if this list does not contain the element.
int lastIndexOfElement2 = arrayList.lastIndexOf(2);//[0,1,2,3,4,5]->The result is 2
//Returns a list between the specified inclusive fromIndex, and exclusive toIndex.(If fromIndex and toIndex are equal, the returned list is empty.)
List<Integer> subArrayList = arrayList.subList(0, 2);//[0,1,2,3,4,5]->[0,1,2]

//Several ways to initialize a set
HashSet<String> hashSet = new HashSet<>() {{
    add("element1");
    add("element2");
}};
//Transfer an ArrayList to a HashSet
hashSet = new HashSet<>(Arrays.asList("element1", "element2"));
hashSet = new HashSet<>(Set.of("element1", "element2"));
hashSet.add("element3");
//Since Java9, Set.of() returns an unmodifiable set
Set<String> immutableSet = Set.of("element1", "element2");
immutableSet.add("element3");//Throws an UnsupportedOperationException
```

### 3.2 Map and HashMap

```java
//Initialize a map
HashMap<String, String> hashMap = new HashMap<>() {{
    put("key1", "value1");
    put("key2", "value2");
}};
String hashMapString = hashMap.toString();//{key1=value1, key2=value2}
hashMap.put("key3", "value3");
//Since Java9, Map.of() returns an unmodifiable map
hashMap = new HashMap<>(Map.of("key1", "value1", "key2", "value2"));
Map<String, String> immutableMap = Map.of("key1", "value1", "key2", "value2");
//*immutableMap.put("key3", "value3");//Throws an UnsupportedOperationException

//Maps common methods
//Removes all the mappings from this map.
hashMap.clear();
//Returns true if this map contains no key-value mappings.
boolean isEmpty = hashMap.isEmpty();
//Associates the specified value with the specified key in this map, if the map previously contained a mapping for the key, the old value is replaced. Returns the previous value associated with key, or null if there was no mapping for key.
String previousAssociatedValue = hashMap.put("key1", "value1");//Returns null as HashMap was cleared before.
//Copies all the mappings from the specified map to this map
hashMap.putAll(Map.of("key2", "value2", "key3", "value3"));
//Returns the number of key-value mappings in this map
int hashMapSize = hashMap.size();//The size of the HashMap is 3
//Returns the value to which the specified key is mapped, or null if this map contains no mapping for the key
String associatedValue = hashMap.get("key2");//Returns "value2"
//Removes the mapping for the specified key from this map if present, returns the previous value associated with key, or null if there was no mapping for key.
String removedPreviousAssociatedValue = hashMap.remove("key3");//Returns "value3"
//Returns a Set view of the keys contained in this map. The set is backed by the map, so changes to the map are reflected in the set, and vice-versa.
Set<String> hashMapKeySet = hashMap.keySet();
//Returns a Collection view of the values contained in this map. The collection is backed by the map, so changes to the map are reflected in the collection, and vice-versa.
Collection<String> hashMapValueCollection = hashMap.values();
//Returns a Set view of the mappings contained in this map. The set is backed by the map, so changes to the map are reflected in the set, and vice-versa.
Set<Map.Entry<String, String>> hashMapEntrySet = hashMap.entrySet();
//Returns true if this map contains a mapping for the specified key.
boolean ifContainsKey = hashMap.containsKey("key1");
//Returns true if this HashMap maps one or more keys to the specified value.
boolean ifContainsValue = hashMap.containsValue("value1");
//Replaces the entry for the specified key only if it is currently mapped to some value, returns the previous value associated with key, or null if there was no mapping for key.
hashMap.replace("key1", "new_value1");
```

## 4 Utility

### 4.1 java.util.Arrays

#### 4.1.1 Mind Mapping of Java Array's common methods

![常用的Java方法和API (3)](Common%20Java%20APIs%20and%20complexity%20analysis.assets/%E5%B8%B8%E7%94%A8%E7%9A%84Java%E6%96%B9%E6%B3%95%E5%92%8CAPI%20(3).png)

#### 4.1.2 Practical Code Demo

```java
package com.test;
import java.util.Arrays;

public class JavaArrayUsage {
    //This statement only declares a integer array variable `a`. It does not yet initialize `a` with an actual array.
    static int[] nullIntArray;

    public static void main(String[] args) {
        //Print "null" as it isn't initialized
        System.out.println(nullIntArray);

        /*"int tempArray[]" is equivalent to "int[] tempArray", but most Java programmers prefer the former style
          because it neatly separates the type int[] (integer array) from the variable name.*/
        int tempArray[] = null;

        //It is legal to have arrays of length 0. Note that an array of length 0 is not the same as null.
        int[] emptyIntArray = new int[0];
        int[] emptyIntArray2 = new int[]{};
        /*"new int[]" is illegal*/
        //int[] emptyIntArray2 = new int[];
        /*Print "[] 0"*/
        System.out.println(Arrays.toString(emptyIntArray) + " " + emptyIntArray.length);
        /*Print "[] 0"*/
        System.out.println(Arrays.toString(emptyIntArray2) + " " + emptyIntArray2.length);

        //"int[] intArray = {1, 2, 3}" is shorthand for "int[] intArray = new int[]{1, 2, 3}"
        int[] intArray = {1, 2, 3};
        int[] intArray2 = new int[]{1, 2, 3};
        /*This statement declares and initializes an array of 3 integers, and every element of intArray3 is 0
          When you create an array of numbers, all elements are initialized with zero.
          Arrays of boolean are initialized with false. Arrays of objects are initialized with the special value null.*/
        int[] intArray3 = new int[3];
        int arrayLength = 3;
        //The array length need not be a constant before initializing:`new int[arrayLength]` creates an array of length arrayLength.
        int[] intArray4 = new int[arrayLength];
        /*Illegal statements*/
        //int[] intArray5 = new int[3]{1, 2, 3};
        //int[] intArray6 = new int[3]{};

        //To find th number of elements of an array, use "array.length"
        for (int i = 0; i < intArray.length; i++) {
            System.out.println(intArray[i]);
        }
        /*The "for each" loop is called the enhanced for loop. The collection expression must be an array or an
          object of a class that implements the Iterable interface, such as ArrayList. iterable [ˈɪtərəbl]a可迭代的
          You should read this loop as "for each j in intArray".*/
        for (int j : intArray) {
            System.out.println(j);
        }
        //Print "[I@45ee12a7"
        System.out.println(intArray);
        /*The call "Arrays.toString(intArray)" returns a string containing the array elements, such as "[1, 2, 3]"*/
        //Print "[1, 2，3]"
        System.out.println(Arrays.toString(intArray));

        /*Use the "copyOf" method in the Arrays class to copy all values of one array into a new array.
          The additional elements are filled with 0 if the array contains numbers, false if the array
          contains boolean values.*/
        int[] luckyNumbers = {1, 2};
        luckyNumbers = new int[]{1, 3};
        int[] copiedLuckyNumbers = Arrays.copyOf(luckyNumbers, luckyNumbers.length * 2);
        //Print "[1, 3, 0, 0]"
        System.out.println(Arrays.toString(copiedLuckyNumbers));

        int[] unsortedArray = {4, 3, 5, 2};
        //"Arrays.sort()" sorts the specified array into ascending numerical order by the dual-pivot Quicksort.
        Arrays.sort(unsortedArray);
        //Print "[2, 3, 4, 5]"
        System.out.println(Arrays.toString(unsortedArray));

        int[] intArray7 = {1, 2, 3, 5};
        /*"Arrays.binarySearch()" searches the specified array of ints for the specified value using the binary search algorithm.
          The array must be sorted prior to making this call.  If it is not sorted, the results are undefined.  If the array contains
          multiple elements with the specified value, there is no guarantee which one will be found. The method returns the index of
          the search key, if it is contained in the array, otherwise returns a negative number, if it isn't contained in the array.*/
        int searchedNumber3Index = Arrays.binarySearch(intArray7, 3);
        //Print "the index of value 3 in array [1, 2, 3, 5] is 2"
        System.out.println("the index of value 3 in array [1, 2, 3, 5] is " + searchedNumber3Index);
        int searchedNumber4Index = Arrays.binarySearch(intArray7, 4);
        //Print "the index of value 4 in array [1, 2, 3, 5] is -4"
        System.out.println("the index of value 4 in array [1, 2, 3, 5] is " + searchedNumber4Index);

        int[] intArray8 = {1, 3, 5};
        int[] intArray9 = {1, 3, 5};
        //Print "false"
        System.out.println(intArray8 == intArray9);
        /*Print "true", "Arrays.equals()" returns true if the two specified arrays of ints are equal to one another. Two arrays are considered
          equal if both arrays contain the same number of elements, and all corresponding pairs of elements in the two arrays are equal.*/
        System.out.println(Arrays.equals(intArray8, intArray9));

        //Multi-dimensional Arrays
        int[][] twoDimenArray = {
                {1, 3, 2},
                {4, 8, 6}
        };
        /*A "for each" loop does not automatically loop through all elements in a two-dimensional array.
          Instead, it loops through the rows, which are themselves one-dimensional arrays.*/
        //Print "1 3 2 4 8 6"
        for (int[] eachRow : twoDimenArray) {
            for (int eachItem : eachRow) {
                System.out.print(eachItem + " ");
            }
        }
        /*Arrays.deepToString() prints a quick-and-dirty(应急的) list of the elements of a multidimensional array*/
        //Print "[[1, 3, 2], [4, 8, 6]]"
        System.out.println(Arrays.deepToString(twoDimenArray));
        /*"Arrays.deepToString() is illegal to one-dimensional array",
          as it requires the incoming parameter of "Object[]" type.*/
        //System.out.println(Arrays.deepToString(intArray8));

        /*Java has no multidimensional arrays at all, only one-dimensional arrays. A two-dimensional array is
          actually an one-dimensional array, whose every element is an one-dimensional array of "Object[]" type.
          So each row's length of a two dimensional-array can be different, this array is called a "Ragged Array".*/
        //It's legal to only assign the row's value of a two-dimensional array.
        int[][] raggedArray = new int[3][];
        /*It's illegal, as array initializer expected.*/
        //int[][] illegalRaggedArray = new int[][];
        /*It's illegal, as the row's value isn't assigned*/
        //int[][] illegalRaggedArray2 = new int[][3];
        int[] firstRow = raggedArray[0];
        raggedArray[0] = new int[]{1, 4};
        raggedArray[2] = new int[]{5, 7, 9};
        //Print "[[1, 4], null, [5, 7, 9]]"
        System.out.println(Arrays.deepToString(raggedArray));
        
        //"Arrays.asList()" returns a fixed-size list supplied by the inputted array, which is commonly used to initialize a list with elements. The fixed-size list's class name is "java.util.Arrays.ArrayList" but not "java.util.ArrayList", so it doesn't support addition and removal method. "Collections.addAll()" is recommended.
        ArrayList<Integer> integerArrayList = new ArrayList<>(Arrays.asList(1, 2));
        System.out.println(integerArrayList); //Output [1, 2]
    }

}
```

### 4.2 Character

```java
Character newLineCharacter = new Character('\n');
boolean isLetter = Character.isLetter('a');
boolean isDigit = Character.isDigit('1');
boolean isWhitespace = Character.isWhitespace(' ');
boolean isUpperCase = Character.isUpperCase('A');
boolean isLowerCase = Character.isLowerCase('a');
char upperCaseLetter = Character.toUpperCase('a');
Character lowerCaseLetter = Character.toLowerCase('A');
//It's same as String.valueOf(char c)
String characterString = Character.toString('s');
```

### 4.3 Collections

```java
ArrayList<Integer> arrayList = new ArrayList<>(Arrays.asList(1, 2));
//Map and Collection(List, Set, Queue) are two different type-container, so Collections Utility is only useful for Collection.
Collections.addAll(arrayList, Arrays.asList(4, 5).toArray(new Integer[0]));
Collections.addAll(arrayList, 4, 5);
Collections.addAll(arrayList, new Integer[]{4, 5});
Collections.addAll(new HashSet<>(), 1, 2);
//Reverses the order of the elements in the specified list, like "123"->"321"
Collections.reverse(arrayList);
//Swaps the elements at the specified positions in the specified list, like "1234"->"1324"
Collections.swap(arrayList, 1, 2);//Such as "1234"->"1324"
//Returns the minimum or maximum element of the given collection, according to the natural ordering of its elements.
int sum = Collections.min(arrayList) + Collections.max(arrayList);
//Replaces all occurrences of one specified value in a list with another, return true if list contained one or more equal elements replaced
boolean isAtLeastReplaceOnce = Collections.replaceAll(arrayList, 2, 6);//Such as "122324"->"166364"
//Randomly permutes the specified list using a default source of randomness
Collections.shuffle(arrayList);
//Sorts the specified list into ascending order, according to the natural ordering of its elements
Collections.sort(arrayList);//Such as "2143"->"1234"
Collections.sort(arrayList, Collections.reverseOrder());//Such as "2143"->"4321"
```

### 4.4 Iterable and Iterator

#### 4.4.1 Definitions and difference

**java.lang.Iterable** represents a collection that can be iterated over using a *for*-each loop. When implementing an *Iterable*, we need to override the *iterator()* method.

```java
public interface Iterable<T> {
    /** Returns an iterator over elements of type {@code T}. */
    Iterator<T> iterator();
    /** @since 1.8 Performs the given action for each element of the Iterable until all elements have been processed.*/
    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
}
```

**java.util.Iterator** represents an interface that can be used to iterate over a collection. When implementing an *Iterator*, we need to override the *hasNext()* and *next()* methods.

```java
package java.util;
public interface Iterator<E> {
    /** Returns {@code true} if the iteration has more elements. */
    boolean hasNext();
    /** Returns the next element in the iteration. */
    E next();
    /** Removes from the underlying collection the last element returned by this iterator */
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }
}
```

#### 4.4.2 Examples from Source Codes

`java.util.Collection` extends `Iterable`, so ArrayList and HashSet implemented the interface method `Iterable.iterator()`. Here is the implementation of `Iterable.iterator()`from ArrayList:

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    /** Returns an iterator over the elements in this list in proper sequence. */
    public Iterator<E> iterator() {
        return new Itr();
    }
    /** An optimized version of AbstractList.Itr */
    private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;
        // prevent creating a synthetic constructor
        Itr() {}
        public boolean hasNext() {
            return cursor != size;
        }
        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }
        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();
            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
        @Override
        public void forEachRemaining(Consumer<? super E> action) {
            //...
        }
        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
}
```

#### 4.4.3 Common Usage

```java
//4 ways to iterate a list
ArrayList<Integer> arrayList = new ArrayList<>(Arrays.asList(1, 2));
//1-For loop
for (int listIndex = 0; listIndex < arrayList.size(); listIndex++) {
    Integer listElement = arrayList.get(listIndex);
}
//2-Enhanced for loop, for each loop
for (Integer listElement : arrayList) {
    Integer tempListElement = listElement;
}
//3-Transfer a list to an array which can be traversed
Integer[] arrayListArray = arrayList.toArray(new Integer[0]);
//4-Collection Iterator
//Collection.iterator() returns an iterator over the elements in this list in proper sequence.
//Iterator.hasNext() returns true if the iteration has more elements.
for (Iterator<Integer> iterator = arrayList.iterator(); iterator.hasNext(); ) {
    //Iterator.next() returns the next element in the iteration, or throws NoSuchElementException if the iteration has no more elements
    Integer listElement = iterator.next();
    //Iterator.remove() removes the last element returned by this iterator. This method can be called only once per call to next, otherwise throws an IllegalStateException
    iterator.remove();
}

//3 ways to iterate a set
HashSet<Integer> hashSet = new HashSet<>(Arrays.asList(1, 2));
//1-Enhanced for loop, for each loop
for (Integer setElement : hashSet) {
    Integer tempSetElement = setElement;
}
//2-Transfer a set to an array which can be traversed
Integer[] setArray = hashSet.toArray(new Integer[0]);
//3-Iterator
for (Iterator<Integer> iterator = hashSet.iterator(); iterator.hasNext(); ) {
    Integer listElement = iterator.next();
    iterator.remove();
}

//4 ways to iterate a map
//1-Iterator
HashMap<String, String> hashMap = new HashMap<>(Map.of("key1", "value1", "key2", "value2"));
for (Iterator<Map.Entry<String, String>> hashMapIterator = hashMap.entrySet().iterator(); hashMapIterator.hasNext(); ) {
    Map.Entry<String, String> hashMapEntry = hashMapIterator.next();
    String hashMapKey = hashMapEntry.getKey();
    String hashMapValue = hashMapEntry.getValue();
    hashMapIterator.remove();
}
Iterator<String> hashMapKeyIterator = hashMap.keySet().iterator();
Iterator<String> hashMapValueIterator = hashMap.values().iterator();
//2-Enhanced for loop, for each loop
for (Map.Entry<String, String> hashMapEntry : hashMap.entrySet()) {
    String hashMapKeyAndValue = hashMapEntry.getKey() + "=" + hashMapEntry.getValue();
}
//3-Lambda, since Java8
hashMap.forEach((key, value) -> {
    String hashMapKeyAndValue = key + "=" + value;
});
```

### 4.5 Comparable and Comparator

#### 4.5.1 Definitions and difference

`java.lang.Comparable`interface imposes a total ordering on the objects of each class that implements it.  This ordering is referred to as the class's natural ordering, and the class's compareTo method is referred to as its natural comparison method.  Lists (and arrays) of objects that implement this interface can be sorted automatically by Collections. sort (and Arrays.sort).  Objects that implement this interface can be used as keys in a sorted map or as elements in a sorted set, without the need to specify a comparator.

```java
public interface Comparable<T> {
    /**
     * Compares this object with the specified object for order. Returns a
     * negative integer, zero, or a positive integer as this object is less
     * than, equal to, or greater than the specified object.
     * It is strongly recommended, but not strictly required that
     * (x.compareTo(y)==0) == (x.equals(y)).
     * @param   o the object to be compared.
     * @return  a negative integer, zero, or a positive integer as this object
     *          is less than, equal to, or greater than the specified object.
     */
    public int compareTo(T o);
}
```

`java.util.Comparator`  is a comparison function, which imposes a total ordering on some collection of objects. Comparators can be passed to a sort method (such as Collections.sort or Arrays.sort) to allow precise control over the sort order. Comparators can also be used to control the order of certain data structures (such as sorted sets or sorted maps), or to provide an ordering for collections of objects that don't have a natural ordering.

```java
public interface Comparator<T> {
    /**
     * Compares its two arguments for order.  Returns a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.
     * @param o1 the first object to be compared.
     * @param o2 the second object to be compared.
     * @return a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.
     */
    int compare(T o1, T o2);
    /**
     * Indicates whether some other object is equal to this comparator. This method is usually implemented by Object, so it's not explicit to be overitten.
     * @param   obj   the reference object with which to compare.
     * @return  {@code true} only if the specified object is also a comparator and it imposes the same ordering as this comparator.
     */
    boolean equals(Object obj);
}
```

#### 4.5.2 Examples from Source Codes

```java
public final class Integer extends Number implements Comparable<Integer> {
    /**
     * Compares two Integer objects numerically.
     * @param   anotherInteger   the Integer to be compared.
     * @return  Returns a negative integer, zero, or a positive integer as the first argument is numerically less than, equal to, or greater than the second.
     */
    public int compareTo(Integer anotherInteger) {
        return compare(this.value, anotherInteger.value);
    }
    public static int compare(int x, int y) {
        return (x < y) ? -1 : ((x == y) ? 0 : 1);
    }
}

public class Collections {
    public static <T> Comparator<T> reverseOrder() {
        return (Comparator<T>) ReverseComparator.REVERSE_ORDER;
    }
    private static class ReverseComparator implements Comparator<Comparable<Object>>, Serializable {
        static final ReverseComparator REVERSE_ORDER = new ReverseComparator();
        public int compare(Comparable<Object> c1, Comparable<Object> c2) {
            return c2.compareTo(c1);
        }
    }
}
```

#### 4.5.3 Common Usage

```java
public static void comparableAndComparatorUsage() {
    Integer[] integerArray = new Integer[]{1, 3, 2};
    Arrays.sort(integerArray);//[1, 2, 3]
    Arrays.sort(integerArray, Collections.reverseOrder());//[3, 2, 1]
    Arrays.sort(integerArray, new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return o1.compareTo(o2);
        }
    });//[1, 2, 3]

    ArrayList<Integer> arrayList = new ArrayList<>(Arrays.asList(1, 3, 2));
    Collections.sort(arrayList);//[1, 2, 3]
    Collections.sort(arrayList, Collections.reverseOrder());//[3, 2, 1]
    Collections.sort(arrayList, new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return (o1 < o2) ? -1 : ((o1.equals(o2)) ? 0 : 1);
        }
    });//[1, 2, 3]

    ArrayList<ComparableBook> comparableBookArrayList = new ArrayList<>() {{
        add(new ComparableBook("Book1", 10));
        add(new ComparableBook("Book3", 30));
        add(new ComparableBook("Book2", 20));
    }};
    Collections.sort(comparableBookArrayList);
    Arrays.sort(comparableBookArrayList.toArray());
    //[ComparableBook{name='Book1', price=10.0}, ComparableBook{name='Book2', price=20.0}, ComparableBook{name='Book3', price=30.0}]

    ArrayList<Book> bookArrayList = new ArrayList<>() {{
        add(new Book("Book1", 10));
        add(new Book("Book3", 30));
        add(new Book("Book2", 20));
    }};
    Collections.sort(bookArrayList, new Comparator<Book>() {
        @Override
        public int compare(Book book1, Book book2) {
            if (book1.price > book2.price) {
                return 1;
            } else if (book1.price < book2.price) {
                return -1;
            } else {
                return 0;
            }
        }
    });
    //[Book{name='Book1', price=10.0}, Book{name='Book3', price=30.0}, Book{name='Book2', price=20.0}]
}

public static class Book {
    public String name;
    public double price;
    public Book(String name, double price) {
        this.name = name;
        this.price = price;
    }
    @Override
    public String toString() {
        return "Book{" + "name='" + name + '\'' + ", price=" + price + '}';
    }
}

public static class ComparableBook implements Comparable<ComparableBook> {
    public String name;
    public double price;
    public ComparableBook(String name, double price) {
        this.name = name;
        this.price = price;
    }
    @Override
    public int compareTo(ComparableBook comparedBook) {
        if (this.price > comparedBook.price) {
            return 1;
        } else if (this.price == comparedBook.price) {
            return 0;
        } else {
            return -1;
        }
    }
    @Override
    public String toString() {
        return "ComparableBook{" + "name='" + name + '\'' + ", price=" + price + '}';
    }
}
```



ArrayList HashSet HashMap 

Java自带数据结构，例如HashMap，Iterator使用和自定义

Java迭代器

Java比较器

- 自然排序：java.lang.Comparable
- 定制排序：java.util.Comparator

相关工具类，例如Arrays,Character,Collections

ArrayList与数组互相转换，ArrayList,Set,HashMap,数组 互相转换，List.of(),Set.of(),Map.of(),Arrays.asList(),Collections.addAll()。





待完善内容：

√Java符号，例如/ 和 %

√去除首尾空白字符trim() strip() isEmpty() isBlank()

√类型转换(任意类型转换为字符串String.valueOf())，String转为int、long、double、字符、字符数组等类型的方法

√String相关正则表达式使用

√分割split()，拼接String.join()，String的其他方法

√StringBuffer和StringBuilder及其相关方法

√BigInteger和BigDecimal

√Math类及其相关方法，向上取整，四舍五入

√相关工具类，例如Arrays,Character,Collections

√Java自带数据结构，例如HashMap，Iterator使用和自定义,ArrayList与数组互相转换，ArrayList,Set,HashMap,数组 互相转换，List.of(),Set.of(),Map.of(),Arrays.asList(),Collections.addAll()。

√Java比较器

- 自然排序：java.lang.Comparable
- 定制排序：java.util.Comparator



____

References:

[CSDN-LeetCode和牛客网的编程题中常用的Java方法和API](https://blog.csdn.net/wq6ylg08/article/details/105077630)

[简书-java 整数除法](https://www.jianshu.com/p/e3f7b47cae30)

[廖雪峰-Java核心类-字符串和编码](https://www.liaoxuefeng.com/wiki/1252599548343744/1260469698963456)

[掘金-从String中移除空白字符的多种方式！？](https://juejin.cn/post/6869562750813929479)

[简书-code point & code unit](https://www.jianshu.com/p/a7db6ac53d57)

[简书-Java中List与数组互相转换](https://www.jianshu.com/p/7eee157f74fc)

[掘金-《java基础》集合类的toArray方法你会用吗？](https://juejin.cn/post/6844904145753735176)

[JianShu-Java常用类笔记](https://www.jianshu.com/p/f90658f249ca)



Regex Reference:

正则表达式自学网站：https://regexlearn.com/

正则表达式30分钟入门教程：https://deerchao.cn/tutorials/regex/regex.htm

正则表达式可视化及实例代码：http://tool.rbtree.cn/

正则表达式可视化解析树及动画：https://blog.robertelder.org/regular-expression-visualizer/

正则表达式时间复杂度为什么为O(n)：[StackOverFlow-What's the Time Complexity of Average Regex algorithms?](https://stackoverflow.com/questions/5892115/whats-the-time-complexity-of-average-regex-algorithms)



BigDecimal Reference:

[LiaoXueFeng-BigDecimal](https://www.liaoxuefeng.com/wiki/1252599548343744/1279768011997217)

[简书-BigDecimal用法](https://www.jianshu.com/p/c3c6cf0dce66)

[ZhiHu-BigDecimal的浮点数运算能保证精度的原理是什么？](https://zhuanlan.zhihu.com/p/71796835)

[StackOverFlow-Setting scale to a negative number with BigDecimal](https://stackoverflow.com/questions/21590590/setting-scale-to-a-negative-number-with-bigdecimal)

[StackOverFlow-Why scale() method of bigDecimal returning negative value when combining with stripTrailingZeros() method?](https://stackoverflow.com/questions/55508186/why-scale-method-of-bigdecimal-returning-negative-value-when-combining-with-st)



Collection Reference:

[CSDN-Java创建初始化List集合的几种方式](https://blog.csdn.net/weixin_47951400/article/details/124289866)

[ZhiHu-ArrayList源码方法小结(一) 常用方法](https://zhuanlan.zhihu.com/p/110287932)

[CSDN-java-List集合初始化的几种方式与一些常用操作-持续更新](https://blog.csdn.net/qq_44695727/article/details/106517147)

[CSDN-java常用知识点整理](https://blog.csdn.net/qq_44695727/article/details/101054712)

[Java工具类Collections方法详解](https://www.jianshu.com/p/c28b1d1ec85d)

[CSDN-Java中的Collections类](https://blog.csdn.net/qq_41571224/article/details/122291550)

[WeChat-HashMap 的 7 种遍历方式与性能分析！(强烈推荐)](https://mp.weixin.qq.com/s/Zz6mofCtmYpABDL1ap04ow)

[ZhiHu-HashMap和HashSet有多少种遍历方式](https://zhuanlan.zhihu.com/p/58290775)

[CSDN-遍历Arraylist的几种方法](https://blog.csdn.net/qq_42013584/article/details/123907536)

[ZhiHu-Java 中iterator和iterable的关系是怎样的？有何意义？](https://www.zhihu.com/question/271757053)

