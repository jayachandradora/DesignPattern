# Design Pattern

## Defination 

Adapter design pattern is one of the structural design patterns and is used so that two unrelated/incompatible interfaces can work together smoothly.
The object, that joins these incompatible interfaces, is called an Adapter. It is also known as Wrapper design pattern.

### Scenario
We all are familiar with adapters in real life. Whenever we want to use the memory card of our camera in the laptop, we can’t do it directly as there’s no port for it.
We first need to get a compatible card reader in which we place our camera’s memory card and then inject it in our laptop. In this case, card reader works as an 
adapter.

### Example

```ruby
interface Pen {
    void write();
}

interface Marker {
    void mark();
}

class BallPen implements Pen {

    @Override
    public void write() {
        System.out.println("Writing....");
    }
}

class PermanentMarker implements Marker {

    @Override
    public void mark() {
        System.out.println("Marking...");
    }
}

class MarkerAdapter implements Pen {

    Marker marker;

    public MarkerAdapter(Marker marker) {
        this.marker = marker;
    }

    @Override
    public void write() {
        marker.mark();
    }
}
public class DesignPattern {
    public static void main(String[] args)
    {
          Marker marker = new PermanentMarker();
          Pen pen = new MarkerAdapter(marker);
          pen.write();
    }
}
```

### Advantages

Adapter Design is very useful for the system integration when some other existing components have to be adopted by the existing system without 
source-code modifications. It allows reusability of existing functionality and flexibility. It allows using a third-party library’s incompatible code in our 
existing codebase. We can’t update the library’s code to make it compatible as it is not possible and we can’t update our source code because 
it will break our own system as other sub-components might already be using it. Hence, we can create an adapter in this case. Voila !!
