---
layout: post
title: 'Extension methods: BigInteger to Binary, Hex, and Octal'
author: 'James Peng'
tags: ['C#']
---

Here is a class containing extension methods to convert BigInteger instances to binary, hexadecimal, and octal strings:

<script src="https://gist.github.com/jia-hong-peng/c47100e422aec3aea1cf.js"></script>

On first glance, these methods may seem more complex than necessary. A bit of extra complexity is, indeed, added to ensure the proper leading zeros are present in the converted strings.

Let's examine each extension method to see how they work:


# BigInteger.ToBinaryString() #

Here is how to use this extension method to convert a BigInteger to a binary string:

~~~csharp
// Convert BigInteger to binary string.
bigint.ToBinaryString();

~~~

The fundamental core of each of these extension methods is the BigInteger.ToByteArray() method. This method converts a BigInteger to a byte array, which is how we can get the binary representation of a BigInteger value:

~~~csharp
var bytes = bigint.ToByteArray();

~~~

Beware, though, the returned byte array is in little-endian order, so the first array element is the least-significant byte (LSB) of the BigInteger. Since a StringBuilder is used to build the output string--which starts at the most-significant digit (MSB)--**the byte array must be iterated in reverse** so that the most-significant byte is converted first.

Thus, an index pointer is set to the most significant digit (the last element) in the byte array:

~~~csharp
var idx = bytes.Length - 1;

~~~

To capture the converted bytes, a StringBuilder is created:

~~~csharp
var base2 = new StringBuilder(bytes.Length * 8);

~~~

The StringBuilder constructor takes the capacity for the StringBuilder. The capacity needed for the StringBuilder is calculated by taking the number of bytes to convert multiplied by eight (eight binary digits result from each byte converted).

The first byte is then converted to a binary string:

~~~csharp
var binary = Convert.ToString(bytes[idx], 2);

~~~

At this point, it is necessary to ensure that a leading zero exists if the BigInteger is a positive value (see discussion above). If the first converted digit is not a zero, and bigint is positive, then a '0' is appended to the StringBuilder:

~~~csharp
// Ensure leading zero exists if value is positive.
if (binary[0] != '0' && bigint.Sign == 1)
{
  base2.Append('0');
}

~~~

Next, the converted byte is appended to the StringBuilder:

~~~csharp
base2.Append(binary);

~~~

To convert the remaining bytes, a loop iterates the remainder of the byte array in reverse order:

~~~csharp
for (idx--; idx >= 0; idx--)
{
  base16.Append(Convert.ToString(bytes[idx], 2).PadLeft(8, '0'));
}

~~~

Notice that each converted byte is padded on the left with zeros ('0'), as necessary, so that the converted string is eight binary characters. This is extremely important. Without this padding, the hexadecimal value '101' would be converted to a binary value of '11'. The leading zeros ensure the conversion is '100000001'.

When all bytes are converted, the StringBuilder contains the complete binary string, which is returned by the extension method:

~~~csharp
return base2.ToString();

~~~

# BigInteger.ToOctalString #

Converting a BigInteger to an octal (base 8) string is more complicated. The problem is octal digits represent three bits which is not an even multiple of the eight bits held in each element of the byte array created by BigInteger.ToByteArray(). To solve this problem, three bytes from the array are combined into chunks of 24-bits. Each 24-bit chunk evenly converts to eight octal characters.

The first 24-bit chunk requires some modulo math:

~~~csharp
var extra = bytes.Length % 3;

~~~

This calculation determines how many bytes are "extra" when the entire byte array is split into three-byte (24-bit) chunks. The first conversion to octal (the most-significant digits) gets the "extra" bytes so that all remaining conversions will get three bytes each.

If there are no "extra" bytes, then the first chunk gets a full three bytes:

~~~csharp
if (extra == 0)
{
  extra = 3;
}

~~~

The first chunk is loaded into an integer variable called int24 that holds up to 24-bits. Each byte of the chunk is loaded. As additional bytes are loaded, the previous bits in int24 are left-shifted by 8-bits to make room:

~~~csharp
int int24 = 0;
for (; extra != 0; extra--)
{
  int24 <<= 8;
  int24 += bytes[idx--];
}

~~~

Conversion of a 24-bit chunk to octal is accomplished by:

~~~csharp
var octal = Convert.ToString(int24, 8);

~~~

Again, the first digit must be a leading zero if the BigInteger is a positive value:

~~~csharp
// Ensure leading zero exists if value is positive.
if (octal[0] != '0' && bigint.Sign == 1)
{
  base8.Append('0');
}

~~~

The first converted chunk is appended to the StringBuilder:

~~~csharp
base8.Append(octal);

~~~

The remaining 24-bit chunks are converted in a loop:

~~~csharp
for (; idx >= 0; idx -= 3)
{
  int24 = (bytes[idx] << 16) + (bytes[idx -1] << 8) + bytes[idx - 2];
  base8.Append(Convert.ToString(int24, 8).PadLeft(8, '0'));
}

~~~

Like the binary conversion, each converted octal string is left-padded with zeros so that '7' becomes '00000007'. This ensures that zeros won't be dropped from the middle of converted strings (i.e., '17' instead of '100000007').

# Conversion to Base x? #

Converting a BigInteger to other number bases could be far more complicated. As long as the number base is a power of two (i.e., 2, 4, 8, 16) the byte array created by BigInteger.ToByteArray() can be appropriately split into chunks of bits and converted.

However, if the number base is not a power of two, the problem becomes much more complicated and requires a good deal of looping and division. As such number base conversions are rare, I have only covered the popular computing number bases here.


# 參考： #

- http://stackoverflow.com/questions/14048476/biginteger-to-hex-decimal-octal-binary-strings
