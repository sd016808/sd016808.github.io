---
layout: post
title: 'C# Refactoring - Composing Methods'
author: 'James Peng'
tags: ['Refactoring']
---

Much of refactoring is devoted to correctly composing methods. In most cases, excessively long methods are the root of all evil. The vagaries of code inside these methods conceal the execution logic and make the method extremely hard to understand – and even harder to change.

The refactoring techniques in this group streamline methods, remove code duplication, and pave the way for future improvements.

## Extract Method ##

**Problem**

~~~csharp
void PrintOwing() 
{
  PrintBanner();

  //print details
  Console.WriteLine("name: " + name);
  Console.WriteLine("amount: " + GetOutstanding());
}
~~~

**Solution**

~~~csharp
void PrintOwing()
{
  PrintBanner();
  PrintDetails(GetOutstanding());
}

void PrintDetails(double outstanding)
{
  Console.WriteLine("name: " + name);
  Console.WriteLine("amount: " + outstanding);
}
~~~

----------


## Inline Method ##

**Problem**

~~~csharp
class PizzaDelivery 
{
  //...
  int GetRating() 
  {
    return MoreThanFiveLateDeliveries() ? 2 : 1;
  }
  bool MoreThanFiveLateDeliveries() 
  {
    return numberOfLateDeliveries > 5;
  }
}
~~~

**Solution**

~~~csharp
class PizzaDelivery 
{
  //...
  int GetRating() 
  {
    return numberOfLateDeliveries > 5 ? 2 : 1;
  }
}
~~~

----------


## Extract Variable ##

**Problem**

~~~csharp
void RenderBanner() 
{
  if ((platform.ToUpper().IndexOf("MAC") > -1) &&
       (browser.ToUpper().IndexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
~~~

**Solution**

~~~csharp
void RenderBanner() 
{
  readonly bool isMacOs = platform.ToUpper().IndexOf("MAC") > -1;
  readonly bool isIE = browser.ToUpper().IndexOf("IE") > -1;
  readonly bool wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) 
  {
    // do something
  }
}
~~~

----------


## Inline Temp ##

**Problem**

~~~csharp
double HasDiscount(Order order) 
{
  double basePrice = order.BasePrice();
  return (basePrice > 1000);
}
~~~

**Solution**

~~~csharp
double HasDiscount(Order order) 
{
  return (order.BasePrice() > 1000);
}
~~~

----------

## Replace Temp with Query ##

**Problem**

~~~csharp
double CalculateTotal() 
{
  double basePrice = quantity * itemPrice;
  
  if (basePrice > 1000) 
  {
    return basePrice * 0.95;
  }
  else 
  {
    return basePrice * 0.98;
  }
}
~~~

**Solution**

~~~csharp
double CalculateTotal() 
{
  if (BasePrice() > 1000) 
  {
    return BasePrice() * 0.95;
  }
  else 
  {
    return BasePrice() * 0.98;
  }
}
double BasePrice() 
{
  return quantity * itemPrice;
}
~~~

----------

## Split Temporary Variable ##

**Problem**

~~~csharp
double temp = 2 * (height + width);
Console.WriteLine(temp);
temp = height * width;
Console.WriteLine(temp);
~~~

**Solution**

~~~csharp
readonly double perimeter = 2 * (height + width);
Console.WriteLine(perimeter);
readonly double area = height * width;
Console.WriteLine(area);
~~~

----------

## Remove Assignments to Parameters ##

**Problem**

~~~csharp
int Discount(int inputVal, int quantity) 
{
  if (inputVal > 50) 
  {
    inputVal -= 2;
  }
  //...
}
~~~

**Solution**

~~~csharp
int Discount(int inputVal, int quantity) 
{
  int result = inputVal;
  
  if (inputVal > 50) 
  {
    result -= 2;
  }
  //...
}
~~~

----------


## Replace Method with Method Object ##

**Problem**

~~~csharp
public class Order 
{
  //...
  public double Price() 
  {
    double primaryBasePrice;
    double secondaryBasePrice;
    double tertiaryBasePrice;
    // long computation.
    //...
  }
}
~~~

**Solution**

~~~csharp
public class Order 
{
  //...
  public double Price() 
  {
    return new PriceCalculator(this).Compute();
  }
}

public class PriceCalculator 
{
  private double primaryBasePrice;
  private double secondaryBasePrice;
  private double tertiaryBasePrice;
  
  public PriceCalculator(Order order) 
  {
    // copy relevant information from order object.
    //...
  }
  
  public double Compute() 
  {
    // long computation.
    //...
  }
}
~~~

----------

## Substitute Algorithm ##

**Problem**

~~~csharp
string FoundPerson(string[] people)
{
  for (int i = 0; i < people.Length; i++) 
  {
    if (people[i].Equals("Don"))
    {
      return "Don";
    }
    if (people[i].Equals("John"))
    {
      return "John";
    }
    if (people[i].Equals("Kent"))
    {
      return "Kent";
    }
  }
  return String.Empty;
}
~~~

**Solution**

~~~csharp
string FoundPerson(string[] people)
{
  List<string> candidates = new List<string>() {"Don", "John", "Kent"};
  
  for (int i = 0; i < people.Length; i++) 
  {
    if (candidates.Contains(people[i])) 
    {
      return people[i];
    }
  }
  
  return String.Empty;
}
~~~

----------

參考:

- https://sourcemaking.com/refactoring/