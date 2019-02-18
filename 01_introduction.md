# Introduction
* This tutorial is the next step after the ` ReactJS Basics ` tutorial.
* In ` ReactJS Basics ` tutorial we saw how managing ` state ` in complex applications that have many **components** nested in each other may lead to a problem.
    * When a **Root** ` Component ` has ` state ` that we need to update from **children** ` Component ` we have to pass the data in a chain from children to their parents until we reach the **Root** ` Component `.
    * What's worse is that when a **child** ` Component ` wants to communicate with one of its **siblings**, we need to pass the data from child to parent then from parent to child which leads to difficulties in managing the ` state ` when we have multiple nested **components**.
* This makes the application **unmanageable** as well as **unscalable**. For that issue ` Redux ` comes to provide a better **solution** for managing ` state ` and communication between **Components** and each other in a really easy and manageable way.
