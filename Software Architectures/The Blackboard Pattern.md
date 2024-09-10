---
resource:
---
## **Introduction**
The Blackboard pattern is often applied to problem scenarios where a system gets data input in a continuous stream and the system needs to make some decisions or solve a problem (or, at least, partially solve the problem). The classic example that is often cited for the Blackboard pattern is speech recognition ([ref#2](https://www.researchgate.net/publication/230875978_The_Hearsay_II_Speech_Understanding_System)). Another example where the Blackboard pattern is often used is the multi-sensor environment where the system responds autonomously to various sensor inputs ([ref#5](http://hillside.net/plop/plop97/Proceedings/lalanda.pdf)). With the advent of IoT ecosystems, it is easy to see the applicability of this pattern where data streams in from multiple sensors and the system need to react based on the characteristics of the input data. In an autonomous vehicle navigation scenario, the system gets input continuously from various sensors and need to make driving decisions in a continuous fashion. For simplicity of arguments, I consider an autonomous vehicle (AV) that needs to adjust its velocity based on velocities of a vehicle in the front of them and the velocity of a vehicle in the right lane.
## Java and the Blackboard Pattern
The framework has three major abstractions – Blackboard, controller, and one or more knowledge sources.  The following diagram illustrates their interactions.

![Blackboard Pattern Components](https://dz2cdn1.dzone.com/storage/temp/7702241-blackboard-pattern-components.png "Blackboard Pattern Components")

The Blackboard receives input (`BlackBoardObjects`) in a continuous stream. The Blackboard (an `Observable`) notifies the controller (an `Observer`) whenever it receives a Blackboard object. The controller enrolls a knowledge source — that can handle the task — whenever it is notified about a Blackboard object. The knowledge sources run in their own thread, process the Blackboard object, and update the Blackboard with a “partial solution Blackboard object.” The solution process continues with the Blackboard notifying the controller of the “partial solution Blackboard object” and so forth. When the controller receives a Blackboard object whose `isReady` flag is true, then the controller executes a solution step.
For brevity of discussion, I am using Java’s `Observable` and `Observer` classes to illustrate the pattern.
The Java classes for the framework include:
**Blackboard.java**
When a new `BlackBoardObject` is added, the `BlackBoard` notifies the `controller`:
```
public interface BlackBoard {

       public void addBlackBoardObject(BlackBoardObject bbo);

       public void notifyController(BlackBoardObject bbo);
}
```
**AbstractBlackBoard.java**
```
public abstract class AbstractBlackBoard extends Observable implements BlackBoard {
     public void addBlackBoardObject(BlackBoardObject bbo) {

          setChanged();
          notifyController(bbo);
     }

     public void notifyController(BlackBoardObject bbo) {
          notifyObservers(bbo);
     }
}
```
**BlackBoardController.java**

When the `BlackBoardController` is updated with a new `BlackBoardObject`, it queries the `KnowledgeSource` objects to see who might be interested in handling the task. It then assigns the `BlackBoardObject` to the `KnowledgeSource` and spawns a new thread for the task. When the controller receives a `BlackBoardObject` with the flag `isReady` set to true, it calls the`execOutcome(BlackBoardObject bbo)` method:

```
public interface BlackBoardController extends Observer {

     public void setKnowledgeSourceList(List<KnowledgeSource> ksList);

     public void enrollKnowledgeSource(KnowledgeSource ks, ExecutorService exsvc);

     public void execOutcome(BlackBoardObject bbo);

}
```

**AbstractBlackBoardController.java**
```
public abstract class AbstractBlackBoardController implements BlackBoardController {

     protected List<KnowledgeSource> ksList = new ArrayList<KnowledgeSource>();

     public void update(Observable bb, Object bbo) {

          ExecutorService exsvc = Executors.newFixedThreadPool(1);

          if (((BlackBoardObject) bbo).isReady())
               execOutcome((BlackBoardObject) bbo);
          else {
               for (KnowledgeSource ks : ksList) {
                    if (ks.canHandle((BlackBoardObject) bbo, (AbstractBlackBoard) bb)) {
                         enrollKnowledgeSource(ks, exsvc);
                         break;
                    }
               }
          }

          execsvc.shutdown();
     }

     public void setKnowledgeSourceList(List<KnowledgeSource> ksList) {
          this.ksList = ksList;
     }

     public void enrollKnowledgeSource(KnowledgeSource ks, ExecutorService exsvc) {
          exsvc.ex
```
**BlackBoardObject.java**
These are the basic data units that are placed on the Blackboard. The `BlackBoardObject` has a boolean flag `isReady` to indicate whether a decision point has been reached.

```
public interface BlackBoardObject {
     public boolean isReady();
}
```

**AbstractBlackBoardObject.java**

```java
public abstract class AbstractBlackBoardObject implements BlackBoardObject {

     protected boolean isReady;

     public boolean isReady() {
          return isReady;
     }

     public void setReady(boolean isReady) {
          this.isReady = isReady;
     }

}
```
**KnowledgeSource.java**
The `KnowledgeSource` objects transform one kind of `BlackBoardObject` to another kind and update the `BlackBoard`. Optionally, the `KnowledgeSource` can set the `isReady` flag to true so the `controller` can execute a reaction to the input received so far. Keep in mind: A `KnowledgeSource` can operate in its own thread.

```java
public interface KnowledgeSource extends Runnable {

     public boolean canHandle(BlackBoardObject bbo, BlackBoard bb);

     public BlackBoardObject process(BlackBoardObject bbo) throws Exception;

     public void updateBlackBoardObject(BlackBoardObject bbo);

}
```
**AbstractKnowledgeSource.java**

```
public abstract class AbstractKnowledgeSource implements KnowledgeSource {

     protected BlackBoardObject bbo;
     protected BlackBoard bb;

     public void run() {
          try {
              updateBlackBoardObject(process(bbo));
          }catch(Exception ex) {
              //TODO: log the exception
          }
     }

     public void updateBlackBoardObject(BlackBoardObject bbo) {
          bb.addBlackBoardObject(bbo);
     }

```
​
## Autonomous Vehicle Navigation Example

The source can be downloaded from [GitHub](https://github.com/mapteb/blackboard-pattern-for-autonavigation).  I have chosen a "Hello-World" style autonomous vehicle navigation scenario to illustrate the usage of the framework.   

Consider a simplified autonomous vehicle (AV) navigation scenario where the AV is at a steady speed but needs to be able to decide whether or not the current speed, at any given point in time, must be altered based on the velocity input of the front vehicle (FV) and velocity input of the right lane vehicle (RLV).  

1. The AV gets the input data stream from two sensors – FV sensor and RLV sensor, which are added to the `AutoNavBlackBoard` as `FrontVehicleDataBBO` and `RightVehicleDataBBO`.
2. Upon receiving the BBOs, Blackboard notifies the `AutoNavBBController`
3. The controller then enrolls `FrontVehicleDataKS` and `RightLaneVehicleDataKS` to handle these BBOs
4. These knowledge sources analyze the data, compute a delta velocity and place a `DeltaVelocityDataBBO` on the Blackboard.
5. Blackboard notifies the controller of `DeltaVelocityDataBBO`
6. The controller then enrolls a third knowledge source `DeltaVelocityDataKS`, which transforms `DeltaVelocityDataBBO` into a `BrakePedalBBO`, sets the `isReady` flag to true (steering wheel and accelerator pedal changes are ignored here for the brevity of this discussion), and updates the Blackboard.
7. When the controller receives any BBO with the flag`isReady` set to true, it calls its `execOutcome(BlackBoardObject bbo)`  method, which then operates the brake pedal.

When the code is run, we get the following output:

```shell
==>> Blackboard received BBO rnd.pattern.blackboard.autonav.bbo.FrontVehicleDataBBO
==>> Blackboard received BBO rnd.pattern.blackboard.autonav.bbo.RightLaneVehicleDataBBO
==>> FrontVehicleDataKS processed FrontVehicleDataBBO
==>> Blackboard received BBO rnd.pattern.blackboard.autonav.bbo.DeltaVelocityDataBBO
==>> DeltaVelocityDataKS processed DeltaVelocityDataBBO
==>> Blackboard received BBO rnd.pattern.blackboard.autonav.bbo.BrakePedalBBO
==>> Operating brake pedal
==>> RightLaneVehicleDataKS processed RightLaneVehicleDataBBO
==>> Blackboard received BBO rnd.pattern.blackboard.autonav.bbo.DeltaVelocityDataBBO
==>> DeltaVelocityDataKS processed DeltaVelocityDataBBO
==>> Blackboard received BBO rnd.pattern.blackboard.autonav.bbo.BrakePedalBBO
==>> Operating brake pedal...
```
In summary, here are the major takeaways from this demonstration:
- The system can be easily extended by adding more knowledge sources like `PotHoleDataKS`, `TrafficSignalDataKS`, etc.
- In the framework discussed above, I have used JDK’s `Observer` and `Observable` classes for the brevity of discussion. Since these classes are deprecated in Java 9, the reader may consider using alternatives like the Java 9 [Flow](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Flow.html) API, etc.
- Currently, the controller is not receiving any feedback from the knowledge sources. It would be interesting to modify the controller to receive feedback and then iterate on the task submissions to intelligently react (see, for instance, [ref#4](http://natureofcode.com/book/chapter-10-neural-networks)).


---
https://dzone.com/articles/the-blackboard-pattern-for-autonomous-navigation?authuser=0