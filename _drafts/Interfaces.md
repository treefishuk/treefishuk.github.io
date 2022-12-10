---
layout: post
title: Whats the deal with interfaces?
--- 

I was recently asked if I could help describe interfaces in c# and how they are used. So this is an attempt to provide some insight on that front. Le me start with...

## What is an interface?

An interface is like a bluprint for a class. Stating the properties and methods that inheriting classes must implement.

For example a car and bike dealership might have an interface that returns the spec on a vehicle:

```
    public interface IVehicle
    {
        string LiscencePlate { get; }

        string ManufacturerName { get; }

        int MaxSpeed { get; }

        int WheelCount { get; }

        string Type { get; }
    }
```

And the different types of vehicle would implement that interface:

```
    public class Car : IVehicle
    {
        public Car(string liscencePlate, string manufacturerName, int maxSpeed)
        {
            LiscencePlate = liscencePlate;
            ManufacturerName = manufacturerName;
            MaxSpeed = maxSpeed;
        }

        public string LiscencePlate { get; private set; }

        public string ManufacturerName { get; private set; }

        public int MaxSpeed { get; private set; }

        public int WheelCount => 4;

        public string Type => "Car";
    }

    public class Bike : IVehicle
    {
        public Bike(string liscencePlate, string manufacturerName, int maxSpeed)
        {
            LiscencePlate = liscencePlate;
            ManufacturerName = manufacturerName;
            MaxSpeed = maxSpeed;
        }

        public string LiscencePlate { get; private set; }

        public string ManufacturerName { get; private set; }

        public int MaxSpeed { get; private set; }

        public int WheelCount => 2;

        public string Type => "Bike";
    }

```

Then code that accepts an interface of vehicle can be passed either a bike or a car and it will work fine. 

```
    internal static class ShowRoomService
    {
        internal static int GetNumberOfWheels(IVehicle vehicle)
        {
            return vehicle.WheelCount;
        }

    }
```
