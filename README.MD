# AsciiPanel

[![Build Status](https://travis-ci.org/roddy/MavenizedAsciiPanel.svg)](https://travis-ci.org/roddy/MavenizedAsciiPanel)

AsciiPanel simulates a [code page 437](https://en.wikipedia.org/wiki/Code_page_437) ASCII terminal display. It supports all 256 characters of codepage 437, arbitrary foreground colors, arbitrary background colors, and arbitrary terminal sizes.

The default terminal size is 80x24 characters and default colors matching the Windows Command Prompt are provided. The default font is 9x16 pixel CP437 (`CP437_9x16`.)

This should be useful to roguelike developers.

## Usage

AsciiPanel supports the customization of fonts. The following system fonts are provided:
- CP437_9x16 
- CP437_8x8 
- CP437_10x10 
- CP437_12x12
- CP437_16x16

In addition, the following fontsets from the [Dwarf Fortress Tileset](http://dwarffortresswiki.org/Tileset_repository) are also included:
- DRAKE_10x10
- QBICFEET_10x10 
- TALRYTH_15x15 

The AsciiPanel class provides a special three-parameter constructor to support font customization. Simply pass the desired AsciiFont as the third parameter as follows.

```java
AsciiPanel myPanel = new AsciiPanel(80, 24, AsciiFont.DRAKE_10x10);

```

## Demo

Small demo of AsciiPanel.
```java
import asciiPanel.AsciiFont;
import asciiPanel.AsciiPanel;

import javax.swing.*;
import java.awt.*;
import java.util.Random;

public class Demo {
    // Declare variables for the AsciiPanel terminal, JFrame, random generator, and the title string
    private static AsciiPanel terminal;
    private static JFrame frame;
    private static Random rand;
    private static String TITLE = "Demo AsciiPanel";

    public static void main(String[] args) {
        // Initialize the frame, terminal, and random number generator
        // JFrame is the container for the terminal
        frame = new JFrame(TITLE);  // Create a new JFrame with the title "Demo AsciiPanel"
        terminal = new AsciiPanel();  // Create a new AsciiPanel instance (the terminal)
        rand = new Random();  // Initialize the random generator for background color selection

        // Write the title in the center of the terminal at row 2
        terminal.writeCenter(TITLE, 2);

        // Write the text "Red color" in red at row 5
        terminal.writeCenter("Red color", 5, AsciiPanel.red);

        // Random generation loop: write spaces with random background colors
        // Iterate through a specific region (from x = 15 to 45, y = 10 to 20)
        for (int x = 15; x < 45; x++) {
            for (int y = 10; y < 20; y++) {
                // Randomly choose a background color (either blue or green)
                if (rand.nextInt(2) == 0) {
                    terminal.setDefaultBackgroundColor(AsciiPanel.blue);  // Set background color to blue
                } else {
                    terminal.setDefaultBackgroundColor(AsciiPanel.green);  // Set background color to green
                }

                // Write a space character (" ") at the current position (x, y) with the selected background color
                terminal.write(" ", x, y);
            }
        }

        // Add the terminal to the frame
        // Set the frame properties: make it non-resizable, pack it to fit the terminal, and set default close operation
        frame.add(terminal);
        frame.setResizable(false);  // Disable resizing of the window
        frame.pack();  // Adjust the window size to fit the terminal size
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  // Ensure the application exits when the window is closed
        frame.setVisible(true);  // Make the window visible to the user
    }
}
```

## Build instructions

AsciiPanel is a [Maven](https://maven.apache.org/) project, compatible with Maven 2 and Maven 3. It can be built using the following command:

```
mvn install
```

This will build the project, run the unit tests, and copy the resulting jar into your local Maven repository. Once the jar is deployed to your repository, you can include it in your projects by including the following dependency in your pom:

```xml
<dependency>
  <groupId>net.trystan</groupId>
  <artifactId>ascii-panel</artifactId>
  <version>1.1</version>
</dependency>
```

Or you can add the jitpack repository to your pom:

```xml
<repository>
  <id>jitpack.io</id>
  <url>https://jitpack.io</url>
</repository>
```

which provides AsciiPanel as dependency at:

```xml
<dependency>
  <groupId>com.github.trystan</groupId>
  <artifactId>AsciiPanel</artifactId>
  <version>31bc98d</version>
</dependency>
```

where `<version/>` describes the git commit you want to use.

For Gradle users:

```gradle
repositories {
    //...
    allprojects {
        repositories {
            maven { url 'https://jitpack.io' }
        }
    }
}

dependencies {
    //...
    compile 'com.github.trystan:AsciiPanel:master-SNAPSHOT'
}
```

## Notes

This project is built with Java 8. However the code itself does not *require* Java 8. If you are supporting a project running an earlier version of Java, you can change the pom file and rebuild the jar using your chosen version of Java without having to modify the code.
