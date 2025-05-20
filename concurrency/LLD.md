https://github.com/shashankChndkr/Low-Level-Design/tree/main/synonym%20sentences
**InShort** - 

* Dont think of data and actions as different.
**Creational**
* Builder - if object has tons of variations, instead of passing null in constructor we can use builder.
**Structural**
* Flywheel - like normalization. If lot of objects have many common data, we can separate common data into a diff object and use it as an attribute. we can move actions to common.
* Adapter - two different interfaces can cross use methods by creating adapter classes.
* Bridge - break a large monolith class into smaller and managable modules.
* Composite - we can create a composite class that interacts with a large number of objects as a single object.
* Decorator - we can enrich objects at runtime using decorators without needing inheritance or stuff.
* Facade - create simple interface for complex libraries.
* Proxy - intermediary layer that can control objects and can perform something before requests gets to the original object.
**Behaviour** 
* Chain of Responsibility - pass requests to handlers for handling sequentially or parallel. like for validation or routing at runtime.
* Iterator - complex DS like graph/tree traversal code can be simplified and reused by moving it into an iterator.
* Observer - pub sub. we have a publisher(eventEmitter) and subscriber(EventListener).
* Command - we can keep an action as an object with all details so that we can process it async or have undo/redo operations.
* Mediator - if we have too many calls between components, we can force them to communicate between mediator only so that the cohesion is reduced.
* Memento - create snapshots so that it can be restored. we have a snapshot class and object class creates snapshot, object class is passed to snapshot and we can restore snapshot by calling restore snapshot.
* State - if a lot of behaviour is changing based on the data stored, we can create new objects that have different actions overriden so that we dont have to have lots of ifelse conditions.
* Strategy - if a class does one function but in different ways at runtime, we can create strategies and have a context class that decides which class to call.
* Template - Algo can be separated into steps and clients can override certain classes and implement some of their own steps.
* Visitor - if we need to add new functionality and we don't wanna modify existing one, we create a visitor class with all handling capability to the existing class. existing class can chose what to call.


#### Factory [Open Close, Single Responsibility]
* Factory -  interface for creating objects in base class and derived classes can alter it. instead of creating objects with new keyword, we push it to an abstract class with concrete creators. this way we can easily extend code to new type of objects if required.
* If we are not sure what type of objects we would be working at runtime, we can have abstract class and users can extend so that we can use that object ()

* Dialog is abstract class, code will be used like Dialog = new WindowsDialog() // which will use windows button and if we want to add new OS, create LinuxDialog and LinuxButton interface, this would keep it ![[Screenshot 2025-03-17 at 12.36.46 PM.png]]
* create interface for all products -> create base class for object creation (can have switch or ifelse conditions) -> create subclasses for each product -> after moving all, if base is empty, make it abstract.

#### Abstract factory - 
	we have a interface for factory and all factories will use this interface so that it can be used.
	
![[Screenshot 2025-03-17 at 12.48.31 PM.png]]![[Screenshot 2025-03-17 at 12.51.17 PM.png]]



#### Builder 
* Create complex objects step by step 
```
DynamoDB mapper = DynamoDBBuilder.region(Region.US).credentials(DefaultCredentials).build();
```
* if we use normal constructor then we would have to use a ton of constructors (one for each type of combination) or pass null/empty values. instead we can use this.

#### ProtoType 
* copy existing objects without making code dependent. some fields can be private so we cannot copy by iterating over fields and this creates coupling as we will have to know the object class.
* solution - we can add a clone method in object itself that handles the cloning part.

### Singleton 
* ensures class has only one instance is created.
* make constructor private and have a static method to create objects 

#### Adapter - 
* Allows two different objects with incompatible interfaces to collaborate
* like circle has radius, square has width. we can use an square_to_circle adapter that extends the circle class and use all functions. 
* like circle.validate(square_to_circle(square)); here we didnt have to modify sqaure or circle for extending feature.

 ![[Screenshot 2025-03-17 at 1.15.53 PM.png]]

#### Bridge - 
* separate code into common modules with abstraction and interfaces so that its easier to maintain
![[Screenshot 2025-03-17 at 1.18.11 PM.png]]
#### Composite - 
we can create a composite class that takes related objects and acts as if the set of objects are one. i.e we just need to call boxes.getPrice() which will iterate over all elements in boxes (can groceries , stationary ) and add their product and return price.
![[Screenshot 2025-03-17 at 1.20.31 PM.png]]

#### Decorator -
*  we can attach new behaviour to objects by placing objects inside special wrapper objects 
* Use the Decorator pattern when you need to be able to assign extra behaviors to objects at runtime without breaking the code that uses these objects.
* Use the pattern when it’s awkward or not possible to extend an object’s behavior using inheritance.
* Many programming languages have the `final` keyword that can be used to prevent further extension of a class. For a final class, the only way to reuse is using the Decorator pattern.
#### Facade -
* Create simple interfaces for complex recurring code this reduces overall coupling. like we dont need to modify application for adding new Compressor or updating AudioMixer.

![[Screenshot 2025-03-17 at 1.31.14 PM.png]]

Flywheel - we store extrinsic properties (which do not change much) in a different object so that we dont need duplicate copies of same data.
![[Screenshot 2025-03-17 at 1.41.58 PM.png]]![[Screenshot 2025-03-17 at 1.43.19 PM.png]]
Proxy - we create an intermediary with same interface as the client and then add special stuff like caching.

![[Screenshot 2025-03-17 at 1.45.17 PM.png]]


## Behavioral Pattern 

#### Chain of Responsibility 
*  pass requests along handlers - sequential / parallel and then get them to validate it. All handlers must implement same interface.
* when your program is expected to **process different kinds of requests in various ways**, but the exact types of requests and their sequences are unknown beforehand
* . Use the pattern when it’s essential to execute several handlers in a particular order.
* Use the CoR pattern when the set of handlers and their order are supposed to change at runtime.

#### Command Pattern 
we create objects/interfaces for actions containign all necessary detail to perform the task so that GUI can be independent of business logic.
* Command pattern when you want to queue operations, schedule their execution, or execute them remotely
* implement reversible operations 
* decouples sender and reciever 
* useful for queing / redo operations.
*![[Screenshot 2025-03-17 at 1.57.43 PM.png]]


#### Iterator 
* we can traverse without knowing underlying impl. if we using complex ds/traversal logic, we can provide an iterator to simplify code and impl.

#### Mediator - 


![[Screenshot 2025-03-17 at 2.06.31 PM.png]]![[Screenshot 2025-03-17 at 2.07.02 PM.png]]

#### Memento to create snapshots for backup and stuff

![[Screenshot 2025-03-17 at 2.11.39 PM.png]]

**Observer** - 
* we can provide a subscription like service so that subscribers are notified on changes.

**State** - 
* if we have too many if else condition on how the object should behave based on its data, we can create another state object which has code to see how to behave instead of maintaining a large number of if else conditions.
* like say i want to notify candidate, i'm allowed to do so only when candidate is onboarded or started. I'll create two states Onboarded and started which will have notify method enabled, but other states like applied, hiringcomplete will have notify return null; 

**Strategy** - 
* we take a class that does something in a lot of different ways and move them into different classes which are interchangable.
* strategy class will have execute method and there will be a context class tat will have getStrategy 
![[Screenshot 2025-03-17 at 2.39.25 PM.png]]

**Template** - 
* Allow algo to be defined as steps in a baseclass and allow subclasses to implement their own steps.
* Like lets say we have a class to process payment , UPI, Net banking and credit cards might have some common logic which can be added in a baseclass and specific logic in their classes.
* Several classes with almost same algo
* Let client change certain steps not whole.


**Visitor/ Double Dispatch** - 
* we add new functionality in different classes intead of same one to prevent breaking changes. 
* we allow the object to determinte what action to take instead of allowing client to do it for them.
* our actual object will have function accept - since object knows best they can chose the best function from visitor class. visitor classes will have have the functions. Like export to PDF, XML, CSV and stuff.
* exporter will have methods on how to act on the classes. objects will pick from this method in their class in an accept method. this accept method takes in  the visitor object.
use when - 
* perform an operation on all elements of a complex object structure
* behaviour makes sense only sometimes.