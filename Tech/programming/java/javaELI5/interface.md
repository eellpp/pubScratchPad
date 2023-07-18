## why use intefaces

[Ref](http://stackoverflow.com/questions/383947/what-does-it-mean-to-program-to-an-interface)

There are some wonderful answers on here to this questions that get into all sorts of great detail about interfaces and loosely coupling code, inversion of control and so on. There are some fairly heady discussions, so I'd like to take the opportunity to break things down a bit for understanding why an interface is useful.

When I first started getting exposed to interfaces, I too was confused about their relevance. I didn't understand why you needed them. If we're using a language like Java or C#, we already have inheritance and I viewed interfaces as a weaker form of inheritance and thought, "why bother?" In a sense I was right, you can think of interfaces as sort of a weak form of inheritance, but beyond that I finally understood their use as a language construct by thinking of them as means of classifying common traits or behaviors that were exhibited by potentially many non-related classes of objects.

For example -- say you have a SIM game and have the following classes:
```java
 class HouseFly inherits Insect {
   void FlyAroundYourHead();
   void LandOnThings();
 }

 class Telemarketer inherits Person {
   void CallDuringDinner();
   void ContinueTalkingWhenYouSayNo();
 }
 ```
 
Clearly, these two objects have nothing in common in terms of direct inheritance. But, you could say they are both annoying.

Let's say our game needs to have some sort of random thing that annoys the game player when they eat dinner. This could be a HouseFly or a Telemarketer or both -- but how do you allow for both with a single function? And how do you ask each different type of object to "do their annoying thing" in the same way?

The key to realize is that both a Telemarketer and HouseFly share a common loosely interpreted behavior even though they are nothing alike in terms of modeling them. So, let's make an interface that both can implement:
```java
 interface IPest {
    void BeAnnoying();
 }

 class HouseFly inherits Insect implements IPest {
   void FlyAroundYourHead();
   void LandOnThings();

   void BeAnnoying() {
     FlyAroundYourHead();
     LandOnThings();
   }
 }

 class Telemarketer inherits Person implements IPest {
   void CallDuringDinner();
   void ContinueTalkingWhenYouSayNo();

   void BeAnnoying() {
      CallDuringDinner();
      ContinueTalkingWhenYouSayNo();
   }
 }
 ```
We now have two classes that can each be annoying in their own way. And they do not need to derive from the same base class and share common inherent characteristics -- they simply need to satisfy the contract of IPest -- that contract is simple. You just have to BeAnnoying. In this regard, we can model the following:
```java
 class DiningRoom {

   DiningRoom(Person[] diningPeople, IPest[] pests) { ... }

   void ServeDinner() {
     when diningPeople are eating,

       foreach pest in pests
         pest.BeAnnoying();
   }
 }
 ```

Here we have a dining room that accepts a number of diners and a number of pests -- note the use of the interface. This means that in our little world, a member of the pests array could actually be a Telemarketer object or a HouseFly object.

The ServeDinner method is called when dinner is served and our people in the dining room are supposed to eat. In our little game, that's when our pests do their work -- each pest is instructed to be annoying by way of the IPest interface. In this way, we can easily have both Telemarketers and HouseFlys be annoying in each of their own ways -- we care only that we have something in the DiningRoom object that is a pest, we don't really care what it is and they could have nothing in common with other.

This very contrived pseudo-code example (that dragged on a lot longer than I anticipated) is simply meant to illustrate the kind of thing that finally turned the light on for me in terms of when we might use an interface. I apologize in advance for the silliness of the example, but hope that it helps in your understanding. And, to be sure, the other posted answers you've received here really cover the gamut of the use of interfaces today in design patterns and development methodologies
