# Design Patterns
Almost all the Design Patterns fall into 3 buckets:
- **Creational Patterns**: Instead of creating objects directly, these patterns give you more flexibility in how objects come into existence. Eg: Singleton, Builder and Factory.
- **Structural Patterns:** Deal with how objects realte to each other. Think of them as blueprints for building larger structures from individual pieces. Eg: Facade, Adapter. 
- **Behavioral Patterns:** Handle communication between objects - how they interact and distribute responsibility. Eg: Strategy (The greatest pattern of all time, always use strategy pattern), Observer.

# Creational Patterns

## Singleton Patterns
- When we require that their only be instance of the class, cause more than that would cause conflicts or defined behavior.
- Eg: Database connection pool, Logger class writing to a Log file.
- Pros:
	- Guaranteed Single Instance -> prevents multiple copies of resource/state -> Controlled Resource Usage
	- Global Access Point -> Easy to use anywhere in application -> Controlled Resource Usage. 
- Cons:
	- Testing Difficulties: can't easily mock for unit tests. Complex test setup
	- Threading Issues: Race conditions during instance creation. Need Thread-Safe implementations
- Some people might say that the Singleton pattern is just a glorified Global variable.

## Builder Pattern
- This is best when there are a lot of optional param for a class. We init the class constructor with all the params, but, we make getter, setter and the build methods so that when a object is created and we only want to use some of params, we can only use those methods and build the object.
- Eg: an HTTPRequest Builder classes.
```Ts
const httpRequest = new RequestBuilder()
.setURL('https://api.example.com') 
.setMethod('POST')
.addHeader('Authorization','Bearer token')
.setTimeout(30000)
.setRetries(3)
.setBody({ name: 'John' })
.build();
```

- always use this if the class has more params than you have fingers. 
- It has almost no downsides, and your future self and co-workers will thank you when they are understand what the params are and don't have to guess what each param is.

## Factory Pattern
- Take a look at this code flow of making user within our webapp:
![[Pasted image 20260202100506.png]]
```Ts
// Messy way
const type = 'admin';
const data = {id: '1', name: 'Bunny'};
let user: User;

if (type === 'admin') {
	user = new AdminUser(data);
} else if (type === 'moderator') {
	user = new ModeratorUser(data);
} else {
	user = new RegularUser(data);
}
```
- Like in our backend, where we are taking care of the auth, we can see that there are different kinds of users: Admins, Moderators, Maintainers, Operatords, etc.
- It would be better to make a Factory class which would take care of the messy new user creation logic to be only be written once, in the factory class, and then only use that classes method to create varous users:
```Ts
// cleaner Factory pattern 
class UserFactory {
	static create(type: 'admin' | 'moderator' | 'regular', data: UserData): User {
		switch (type) {
			case 'admin': return new AdminUser(data); 	
			case 'moderator': return new ModeratorUser(data);
			case 'regular': return new RegularUser(data);
			case 'operator': return new OperatorUser(data);
			case 'maintainer': return new MaintainerUser(data);
			default:
				throw new Error(`Invalid user type: ${type}`);
		}	
	}
}
 
// making a new user:
const cleanUser = UserFactory.create('admin', { id:'1', name: 'Bunny' });
```
- You like a actual factory which produces different types of cars coming out of it, without worrying about how the objects are being created every time a new instance is being created.
- Use Factory Pattern when object creation involves complex setup or when you're creating multiple related types. For simple objects, a regular constructor is fine.
- Pros:
	- Any changes or more features, we would like to add to a particular user, we would just need to update the factory.
	- Makes the code way more maintainable cause, all of the object creation logic is all in the same place
- Cons:
	- Adds another layer of abstraction
	- They are annoying coupled on the factory class.

# Structural Patterns
## Facade Pattern
- just like the name, we put a pretty front to hide all the ugliness of the back.
- It may sound very similar to the Factory pattern, but remember that the Factory pattern is for Object creation. Facade is *structural*, maps how the objects relate to each other.
- Like clicking on a buy button has a lot of complex actions behind it, however, we do not care about all of that. We just want our product.
![[Pasted image 20260203085418.png]]
- 