
Three main parts:
1. scalability --> able to handle millions of user
2. Maintainability --> easy to debug, easy to add new feature without breaking existing feature
3. Re-usability --> not tightly coupled

NOTE: DSA is the brain of an app LLD is the skelaton

Why OOPS:
1. Machine language (0/1)
2. Assembly Level language (MOv A 61H)
	1. Prone to error
	2. not scalable 
	3. tedious
3. Procedural Programming
	1. Functions : we can use
	2. loops : we can use
	3. blocks: we can use
		1. Do this 
		2. then do this (means 1st do this then do this)
4. OOPs
	1. real world modelling
	2. Data security
	3. Highly scalable/Reusable


## Example: Owner owns a car

```java
Car{
	String brand;
	String model;
}

Owner{
	String name;
	Car car;
}
```

## Example: Abstraction
```java
interface Car{
	void startEngin();
	void stopEngin();
	void break();
	void accelerate();
}
```

```java
Class SportCar extend Car{
	String brand;
	String model;
	bool isEngineOn;
	int currentSpeed;
	@Override
	void startEngin(){
		sout("Engin started!");
	}
	@Override
	void stopEngin(){
		sout("Engin stop!");
	}
	@Override
	void break(){
		sout("applied break!");
	}
	@Override
	void accelerate(){
		sout("speed 20km/h!");
	}
}
```

``
```java
Class Main{
	public static void main(String[]args){
		Car car = new SpoartCar();
		car.startEngin();
		car.accelerate();
		car.break();
		car.stopEngin();
	}
}
```