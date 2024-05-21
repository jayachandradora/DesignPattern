# Facade Design Pattern

The Facade design pattern is a structural pattern that provides a simplified interface to a complex subsystem of classes, making it easier to use. Here's an overview of the Facade pattern, along with its key components, characteristics, and an example:

## Facade Design Pattern

- **Intent:** Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

- **Key Components:**
  1. **Facade:** Provides a simplified interface to the complex subsystem. It delegates client requests to appropriate subsystem objects and coordinates their interactions.
  2. **Subsystem Classes:** Represents the various components and functionality of the subsystem. These classes implement the actual functionality required by the client.

- **Characteristics:**
  - Simplifies client usage by providing a single entry point to access complex subsystem functionality.
  - Encapsulates the complexity of the subsystem behind a well-defined interface.
  - Promotes loose coupling between clients and the subsystem, as clients interact with the facade rather than directly with subsystem classes.

## Example

Consider a multimedia player application that consists of multiple subsystems such as audio player, video player, and playlist manager. We can use the Facade pattern to create a `MultimediaPlayerFacade` that provides a unified interface to perform common operations such as play, pause, stop, and skip tracks:

```java

// Subsystem classes
class AudioPlayer {
    public void play() { System.out.println("Audio playing..."); }
    public void pause() { System.out.println("Audio paused."); }
    public void stop() { System.out.println("Audio stopped."); }
}

class VideoPlayer {
    public void play() { System.out.println("Video playing..."); }
    public void pause() { System.out.println("Video paused."); }
    public void stop() { System.out.println("Video stopped."); }
}

class PlaylistManager {
    public void next() { System.out.println("Next track..."); }
    public void previous() { System.out.println("Previous track..."); }
}

// Facade class
class MultimediaPlayerFacade {
    private AudioPlayer audioPlayer;
    private VideoPlayer videoPlayer;
    private PlaylistManager playlistManager;

    public MultimediaPlayerFacade() {
        audioPlayer = new AudioPlayer();
        videoPlayer = new VideoPlayer();
        playlistManager = new PlaylistManager();
    }

    public void play() {
        audioPlayer.play();
        videoPlayer.play();
    }

    public void pause() {
        audioPlayer.pause();
        videoPlayer.pause();
    }

    public void stop() {
        audioPlayer.stop();
        videoPlayer.stop();
    }

    public void nextTrack() {
        playlistManager.next();
    }

    public void previousTrack() {
        playlistManager.previous();
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        MultimediaPlayerFacade player = new MultimediaPlayerFacade();
        player.play();
        player.pause();
        player.nextTrack();
    }
}

```
