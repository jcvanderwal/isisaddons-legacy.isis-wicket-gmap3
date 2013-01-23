isis-wicket-gmap3
=================

Extension for Apache Isis' Wicket Viewer, to render a collection of entities within a map (using [gmap3](https://developers.google.com/maps/documentation/javascript/)).  

### Screenshots

![](images/screenshot-1.png)

![](images/screenshot-2.png)

![](images/screenshot-3.png)


### Dependencies

This component has a dependency in turn on the [wicketstuff-gmap3](https://github.com/wicketstuff/core/wiki/Gmap3) component.  Currently this is only available in SNAPSHOT form, obtained from [Sonatype's OSS repository](https://oss.sonatype.org/content/repositories/snapshots).

### Configuration

In your project's parent `pom.xml`, add to the `<dependencyManagement>` section:

    <dependencyManagement>
        <dependencies>
            ...
            <dependency>
                <groupId>com.danhaywood.isis.wicket.ui.components</groupId>
                <artifactId>danhaywood-isis-wicket-gmap3</artifactId>
                <version>1.0.0-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            ...
        </dependencies>
    </dependencyManagement>

In your project's DOM `pom.xml`, add a dependency on the `applib` module:

    <dependencies>
        ...
        <dependency>
            <groupId>com.danhaywood.isis.wicket.ui.components</groupId>
            <artifactId>danhaywood-isis-wicket-gmap3-applib</artifactId>
        </dependency>
        ...
    </dependencies> 

In your project's webapp `pom.xml`, add a dependency on the `ui` module:

    <dependencies>
        ...
        <dependency>
            <groupId>com.danhaywood.isis.wicket.ui.components</groupId>
            <artifactId>danhaywood-isis-wicket-gmap3-ui</artifactId>
        </dependency>
        ...
    </dependencies> 


### Usage

Make your entity implement `com.danhaywood.isis.wicket.gmap2.applib.Locatable`, so that it provides a `Location` property of type `com.danhaywood.isis.wicket.gmap2.applib.Location`.

If using the JDO objectstore, then this property will need to be annotated as `@Persistent`.  Other objectstores (at least, those that use the facilities of Isis' `ValueSemanticsProvider` API) should require no additional configuration.

For example:

import com.danhaywood.isis.wicket.gmap3.applib.Locatable;
import com.danhaywood.isis.wicket.gmap3.applib.Location;

public class ToDoItem implements Locatable {

    ...

    // {{
    @Persistent
    private Location location;
    
    @MemberOrder(name="Detail", sequence = "10")
    @Optional
    public Location getLocation() {
        return location;
    }
    public void setLocation(Location location) {
        this.location = location;
    }
    // }}

}

You should then find that any collections of entities that have date properties (either returned from an action, or as a parented collection) will be rendered in a map.

### End-user entry of `Location`s

The `Location` value type supports input as a string.  The format is:

     mmm.mmm;nnn.nnn

where:

* `mmm.mmm` is the latitute, and
* `nnn.nnn` is the longitude 