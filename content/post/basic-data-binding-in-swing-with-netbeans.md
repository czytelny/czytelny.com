+++
date = "2013-12-08T18:52:12+01:00"
tags = ["Java", "Netbeans"]
title = "Basic data binding in Swing with Netbeans"

+++

Performing data binding in Swing is pain in the ass. Some time ago I lost couple of hours to make it eventually working. Let’s take a closer look what you need to do to bind your data to view. Not any database just POJO with some field. Maybe there is a better way? I know Java FX offers some cool features on that area but I haven’t tested it yet. Ok, here we go:

## **Requirements**

- No external libraries – just plain Swing
- Netbeans 7.4 IDE
- Simplicity

## **Overview**

Model class is extremally simple:

```java
public class Person {
    private String name;
 
    public Person(String name) {
        this.name = name;
    }
 
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

- Two Swing forms: MainView (with a TextField) & SubForm View (with a Label)
- The entry point App.java where views and dummy data are created.

## **Binding**

What I want to achieve is to bind instance of the Person class to the textField on the MainView and to the label on the SubForm. One common model for two views.

### Changes in the model
- add new field

```java
private final PropertyChangeSupport changeSupport = new PropertyChangeSupport(this);
```

- Add two Property Change listener:

```java
public void addPropertyChangeListener(PropertyChangeListener listener) {
       changeSupport.addPropertyChangeListener(listener);
}
public void removePropertyChangeListener(PropertyChangeListener listener) {
       changeSupport.removePropertyChangeListener(listener);
}
```

Modify field setter method to fire property change event to listeners:

```java
public void setName(String name) {
       String old = this.name;
       this.name = name;
       changeSupport.firePropertyChange("name", old, name);
   }
```

### Changes on the view side
First of all add new field with model class Person and getter. I added also a constructor to initialize Person, but it’s not mandatory, you can set it later on by adding an setter.

```java
public class MainView extends javax.swing.JFrame {
    private Person person;
 
 public MainView(Person person) {
        this.person = person;
        initComponents();
    }
 
 public Person getPerson() {
        return person;
    }
...
}
```

- Next part is related with NetBeans IDE: Right Mouse Button Click on the TextField->Bind->Text. Binding source: Form, Binding expression: ${person.name}. Let’s leave Advanced tab with defaults.
- Similiar action for label: Right Mouse Button Click on the Label->Bind->Text

## **Summary**

That’s all. Compile & enjoy:) In my opinion it is the simplest possible example of data binding.
Sources available on the github: [here](https://github.com/czytelny/DataBindingDemo).