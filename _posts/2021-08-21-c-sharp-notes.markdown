---
layout: post
title: "C# Notes"
date: 2021-08-15 11:00:00 +0100
categories:
---

- [Null Checking](#null-checking)
- [Pattern Matching](#pattern-matching)
- [Version History](#version-history)

## Null Checking

```c#
x = xs?[n]?.Sum() ?? double.NaN;

if (name is null) ...

if (name is not null) ...

_ = name ?? throw new ArgumentNullException(nameof(name));

if(getValue() is var v && v |= null) ...

```

## Pattern Matching

```c#
(open, WaterGateOpen, WaterLevel) switch
{
	(false	,false	,WaterLevel.High	) => false,
	(false	,false	,WaterLevel.Low		) => false,
	(false	,true	,WaterLevel.High	) => false,
	(false	,true	,WaterLevel.Low		) => false,
	(true	,false	,WaterLevel.High	) => true,
	(true	,false	,WaterLevel.Low		) => throw new InvalidOptions(),
	(true	,true	,WaterLevel.Low		) => true,
	(true	,true	,WaterLevel.Low		) => false,
}


vehicle switch
{
	Car c	=> 2.00m,
	Taxi t	=> 3.50m,
	Bus b	=> 5.00m,
	Truck t	=> 10.00,
	{ }		=> throw new ArgumentExeption(message: "Unknown vehicle", paramName: nameof(vehicle)),
	null	=> throw new ArgumentNullException(nameof(vehicle))
};


if (employee.Tax is double and >= 10) ...
if (employee.Tax is (10 or 20) and int tax) ...

```

## Version History

| Version		| Visual Studio					|
|-------------- |------------------------------ |
| C# 9.0		| 2019 version 16.8             |
| C# 8.0        | 2019 version 16.3             |
| C# 7.3        | 2017 version 15.7				|
| C# 7.2		| 2017 version 15.5				|
| C# 7.1		| 2017 version 15.3				|
| C# 7.0        | 2017 version 15.0             |

